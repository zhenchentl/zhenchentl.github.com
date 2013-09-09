---
layout: post
title: python redis的简单封装
summary: python redis的简单封装，日后继续完善。
categories:
- redis
tags:
- python
- redis
---

{% highlight python %}
import redis
class redis_do:
    def __init__(self,redis_host,):
        '''连接redis'''
        self.r = redis.StrictRedis(host=redis_host, port=REDIS_PORT, db=REDIS_DB)
        
    def redis_control_all(self,control,key_name_values,limit_len):
        '''带*的key则逐一获取，无*则直接获取'''
        self.key_name_values=key_name_values
        start_limit=0
        if "*" in self.key_name_values:#key_name带*则调用FOR循环获取所有值
            new_data={}
            newkey=self.r.keys(self.key_name_values)
            for i in newkey:
                get_data=self.redis_control("get","%s"%i,"no")
                new_data.setdefault(i,get_data)
                start_limit+=1
                if start_limit==limit_len:
                    break
        else:
            new_data=self.redis_control("get","%s"%self.key_name_values)
        
        return new_data
        
    def redis_control(self,control,key_name_value,check_exit=""):
        '''获取对应的操作方式获取redis数据'''
        REDIS_TYPE={#封装不同类型的key的操作方式
            "string":{"get":"get","set":"set","del":"delete"},
            "hash":{"get":"hgetall",'hget':'hget',"set":"hset","del":"hdel"},
            "set":{"get":"smembers","del":"srem"},
            "list":{"get":"lrange"}
            }
        
        if "," in key_name_value:#是否有value存在
            key_name_values=key_name_value.split(",")
            key_name=key_name_values[0]#key_name键值
                
            kv=[]
            #将KEYNAME与valuse拆分
            for key_v in key_name_values:
                kv.append("'%s'"%key_v)
            new_kv=",".join(kv)#生成"'name','value'"
        else:
            key_name=key_name_value
            new_kv="'%s'"%key_name_value
        
        #是否需要判定key_name存在
        if check_exit=="":#默认检查
            key_name_exists=self.r.exists(key_name)
        else:
            key_name_exists=True
            
        if key_name_exists:
            key_type=self.r.type(key_name)
            data=""  
            if control=="get" and key_type=="list":
                redis_string="data=self.r.%s(%s,0,100)"%(REDIS_TYPE[key_type][control],new_kv)
            else:
                redis_string="data=self.r.%s(%s)"%(REDIS_TYPE[key_type][control],new_kv)
            try:
                #开始操作，查询返回数据列表，其他则返回操作成功与否的列表
                exec(redis_string)
                new_data=[]
                if control=="get" or control=="hget":
                    if key_type=="hash" and control=="get":
                        for k in data:
                            d_v=''
                            exec("d_v=self.r.hget('%s','%s')"%(key_name,k))
                            new_data.append('%s:%s'%(k,d_v))
                        return new_data
                    else:
                        new_data.append(data)
                        return new_data
                else:
                    new_data.append("%s is OK!"%redis_string)
                    return new_data
            except:
                new_data.append("%s is ERROR!"%redis_string)
                return new_data
        else:
            new_data.append("ERROE %s is not exists!"%key_name)
            return new_data
{% endhighlight %}