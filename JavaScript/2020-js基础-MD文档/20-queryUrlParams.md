
## queryUrlParams
>
    /*queryUrlParams:获取URL中？后传参的信息和哈希值。
    @params
        url[string] 要解析的字符串
    @return
        [object] 包含参数和哈希值的对象
    by SHAO on 2020/7/30
     */

```javascript
   function queryUrlParams(url){
        let result={};
        let paramsIn=url.indexOf('?');        //？位置，用于判断是否有参数
        let hashIn=url.indexOf('#');        //# 位置，用于判断是否有哈希值
        let paramsText='';
        let hashText='';

        //没有#时处理,让#所在索引为url的长度，便于对参数内容进行定位截取。以及hash值处理.
        hashIn === -1 ? (hashIn=url.length,hashText=null) : hashText=url.substring(hashIn+1);
        //?存在时处理,以及参数信息字符串获取.
        paramsIn >=0 ? paramsText=url.substring(paramsIn+1,hashIn) : null ;
        
        //信息处理,返回对象
        if(paramsText!==''){
            let paramsAry=paramsText.split('&');
            paramsAry.forEach((item,i)=>{
                let ary = item.split('=');
                result[ary[0]]=ary[1];
            })
        }
        hashText!=='' ? result["HASH"]=hashText : null ;
        
        return result;
    }

    let url='http://www.ishaoqi.com?name=harry&age=20&sex=male#person';
    let paramsObj=queryUrlParams(url);
    console.log(paramsObj);
```

### 正则简化版

```javascript   
    function queryUrlParams(url){
        let result={},
            reg1=/([^?=&#]+)=([^?=&#]+)/g,
            reg2=/#([^?=&#]+)/g;
        url.replace(reg1,(n,x,y)=>result[x]=y);
        url.replace(reg2,(n,x)=>result["HASH"]=x);
        return result;
    }

    let url='http://www.ishaoqi.com?name=harry&age=20&sex=male#person';
    let paramsObj=queryUrlParams(url);
    console.log(paramsObj);
```
