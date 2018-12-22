---
layout: post
title:  "內科床位佔用率\n（按日熱量圖）"
desc: "於服務高峰期期間，醫管局每日發布的內科住院病床使用率數據。"
tags: [醫療服務實況]
img: pic01.jpg
level: 第一級
---

<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.1.0/jquery.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery-sheetrock/1.1.4/dist/sheetrock.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/moment.js/2.20.1/moment.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/moment.js/2.20.1/locale/zh-hk.js"></script>
<script src="https://code.highcharts.com/highcharts.js"></script>
<script src="https://code.highcharts.com/modules/heatmap.js"></script>
<script src="https://code.highcharts.com/modules/boost.js"></script>

<div id="container" style="min-height: 3500px; min-width: 440px; max-width: 880px; margin: 0 auto"></div>


[原始數據](https://docs.google.com/spreadsheets/d/e/2PACX-1vRpbqc-2MwM-s9JtgXKFbfNmNOaTkve2rPmUxZvMoiJdYTJENStLX1W6i47mb-RURj3Or2oXRjPLhgD/pubhtml?gid=0&amp;single=true&amp;widget=true&amp;headers=false)

數據等級：[第一級](/faq/#datalevel)

資料來源：[政府一站通搜尋功能 www.search.gov.hk](http://www.search.gov.hk/result?query="occupancy+rates"+"medical+wards"+"statistics"+"public hospitals"&search_but=&ui_charset=utf-8&web=this&output=xml_no_dtd&client=depts&proxystylesheet=ogcio_home_adv_frontend&ui_lang=en&r_lang=&gp1=gia_home&gp0=gia_home&web=this&txtonly=0&tpl_id=stdsearch&oe=UTF-8&ie=UTF-8&sort=date%3AS%3AS%3Ad1&site=gia_home&num=50)
  
<script>
var hospCats = ["","東區尤德夫人那打素醫院","律敦治醫院","瑪麗醫院","廣華醫院","伊利沙伯醫院","將軍澳醫院","基督教聯合醫院","明愛醫院","瑪嘉烈醫院","仁濟醫院","雅麗氏何妙齡那打素醫院","北區醫院","威爾斯親王醫院","博愛醫院","屯門醫院"];
var dateCats = [];
var chart = new Highcharts.Chart({
        chart: {
            renderTo: 'container',
            type: 'heatmap',
            marginTop: 150,
            marginBottom: 15,
        },
    loading: {
        labelStyle: {
            color: 'white'
        },
        style: {
            backgroundColor: 'gray'
        }
    },
    boost: {
        enabled: true,
	useGPUTranslations: true,
	allowForce: true,
	useAlpha: false,
    },

    yAxis: {
        categories: dateCats,
	reversed:true,
	title:null
    },
    
        credits: {
        enabled: false
    },
    
    title: {
    	text:''
    },

    xAxis: {
        categories: hospCats,
	opposite: true,
        labels: {
            rotation: 90
        }
    },

    colorAxis:{min:80,stops:[[0,'#FFEBEE'],[0.5,'#F44336'],[1,'#B71C1C']]},

    legend: {
        enabled: false
    },

    tooltip: {
        useHTML: true,
	shared: true,
	shadow: false,
	animation: false,
	formatter: function () {
            return '<b>' + this.series.xAxis.categories[this.point.x] + '</b> was <br><b>' +
                this.point.value + '%</b> occupied on <br><b>' + this.series.yAxis.categories[this.point.y] + '</b>';
        }
    },

    series: [{
	boostThreshold: 1,
	turboThreshold: 10,
        data: [],
    dataLabels: {
      enabled: true,
      allowOverlap: true,
      shadow: false,
      optimizeSpeed: true,
      style: {
        textOutline: null,
        color: 'white'
      }
    },
    }]

});

  var data = {{ site.data.MEDOCCUPANCY | jsonify }};
      
      for (var i = 1; i < data.length; i++){
	dateCats[i] = moment(data[i][0]).format("M月D日");
      	for (var j = 1; j < data[i].length; j++){				// ignore first column
      		chart.series[0].addPoint([j,i,parseInt(data[i][j])],false,false);
	}
	}
	console.log(Date.now()+" finished loading data");
	chart.redraw();
	chart.hideLoading();
	console.log(Date.now()+" loading hidden");
      //getData(500);
		
	chart.showLoading();
</script>