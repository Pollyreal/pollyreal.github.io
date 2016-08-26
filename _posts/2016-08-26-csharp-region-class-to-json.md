---
date: 2016-08-26 11:31:31+00:00
layout: post
title: c#动态获取地址列表返回Json
categories: C#
tags: C# Json asp.net
---

需要从数据库动态获取省市区信息，实现了从数据库读取，并将实例转换为json格式返回。  
创建相应格式的class，创建实例并填充数据，转换格式即可。  
```
/// <summary>  
/// 动态获取地址列表  
/// </summary>  
/// <returns></returns>  

[HttpPost]  
public JsonResult GetAddress()  
{  
    var provinceList = ApiHelper.Get("/region/getProvince");  
    var prLength = provinceList.Length;  
    List<Models.City> finalList = new List<Models.City>();  
    //所有省  
    while (prLength != 0)  
    {  
        var prIndex = prLength - 1;  
        Models.City province = new Models.City();  
        //省名  
        province.name = provinceList[prIndex].name;  
        //类型为省  
        province.type = 1;  
        //获取省属市  
        var cityList = ApiHelper.Get("/region/getCityByProvince?id=" + provinceList[prIndex].id);  
        var ctLength = cityList.Length;  
        for (var ctIndex = 0; ctIndex < ctLength; ctIndex++)  
        {  
            Models.City city = new Models.City();  
            city.name = cityList[ctIndex].name;  
            //类型为市  
            city.type = 0;  
            //获取市属区  
            var regionList = ApiHelper.Get("/region/getRegionByParent?id=" + cityList[ctIndex].id);  
            var rgLength = regionList.Length;  
            for (var rgIndex = 0; rgIndex < rgLength; rgIndex++)  
            {  
                Models.City region = new Models.City();  
                region.name = regionList[rgIndex].name;  
                if (rgIndex == 0) city.sub = new List<Models.City>();  
                city.sub.Add(region);  
            }  
            if (ctIndex == 0) province.sub = new List<Models.City>();  
            province.sub.Add(city);  
        }  
        finalList.Add(province);  
        prLength--;  
    }  
    return Json(finalList);  
}  
```
//省市区结构  
```
namespace Models  
{  
    public class City  
    {  
        public string name { get; set; }  
        public int ? type{ get; set; }  
        public virtual List<City> sub { get; set; }  
    }  
}   
```

//前端获取相应json  
```
$.ajax({  
    url:"/user/getaddress",  
    dataType:"json",  
    type:"post",  
    async:false,  
    success: function (result) {  
        console.log(result);  
        a.smConfig.rawCitiesData = result;  
    }});   
```        
