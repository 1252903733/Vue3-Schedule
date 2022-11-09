<template>
  <el-row class="mb-0">
    <el-button type="primary" style="height: 100px;font-size: 40px" @click="lastweekfuntion">last week</el-button>
    <el-button type="success" style="height: 100px;font-size: 40px" @click="thisweekfuntion">this week</el-button>
    <el-button type="info"  style="height: 100px;font-size: 40px" @click="nextweekfuntion">next week</el-button>
    <el-button  v-model="message" style="height: 100px;font-size: 40px;background-color: black"> Current Week: Week {{currentclasstime.time}},{{currentclasstime.data}}</el-button>
  </el-row>


  <table style="border: 1px solid #ccc">
    <tr v-for="row in dateandtime" :key="row.rows">
      <td  v-for="item in row" style="border: 1px solid #ccc;width: 200px;height: 80px" :key="item.cols">
        <!--
                 这个当前显示月日-->
        <div v-if="item.message===currentclasstime.dateandtime">
          <div style="text-align:center;line-height:100px;background-color: red;width: 300px;height: 80px;font-size: 30px;font-weight: bold" >{{item.message}}(today)</div>
        </div>


        <div v-else-if="row.rows===0&&item.cols!==0">
          <div style="text-align:center;line-height:100px;background-color: #2eff87;width: 300px;height: 80px;font-size: 50px;font-weight: bold" >{{item.message}}</div>
        </div>


        <div v-else-if="row.rows===1&&item.cols!==0">
          <div style="text-align:center;line-height:100px;background-color: #29c6ea;width: 300px;height: 80px;font-size: 50px;font-weight: bold" >{{item.message}}</div>
        </div>


        <div v-else-if="row.rows!==1&&row.rows!==0&&item.cols===0">
          <div style="text-align:center;line-height:100px;background-color: #a86eea;width: 250px;height: 160px;font-size: 20px;font-weight: bold" >{{item.message}}</div>
        </div>


      </td>
    </tr>



  </table>
</template>


<script>
import {onMounted, reactive, toRefs} from "vue";

export default {
  setup() {
    const data = reactive({
      dateandtime:"",
      currentclasstime: {
        date: "",
        data: "",
        time: 1,
        dateandtime: ""
      },
    })

    onMounted(async () => {
          showdate()
    }
    )
    const showdate = () => {
      data.dateandtime = []
      for (let i = 0; i < 7; i++) {
        data.dateandtime[i] = {}
        data.dateandtime[i]['rows'] = i
        for (let j = 0; j < 8; j++) {
          data.dateandtime[i][j] = {}
          data.dateandtime[i][j]['cols'] = j
          data.dateandtime[i][j]['message'] = ""
          data.dateandtime[i][j]['coursename'] = ""
          data.dateandtime[i][j]['teachername'] = ""
          data.dateandtime[i][j]['location'] = ""
          data.dateandtime[i][j]['remark'] = ""
        }
      }
      data.dateandtime[1][1]['message'] = "Monday"
      data.dateandtime[1][2]['message'] = "Tuesday"
      data.dateandtime[1][3]['message'] = "Wednesday"
      data.dateandtime[1][4]['message'] = "Thursday"
      data.dateandtime[1][5]['message'] = "Friday"
      data.dateandtime[1][6]['message'] = "Saturday"
      data.dateandtime[1][7]['message'] = "Sunday"
      data.dateandtime[2][0]['message'] = "(8:30-10:25)"
      data.dateandtime[3][0]['message'] = "(10:30-12:00)"
      data.dateandtime[4][0]['message'] = "(14:00-16:25)"
      data.dateandtime[5][0]['message'] = "(16:30-18:00)"
      data.dateandtime[6][0]['message'] = "(19:30-21:00)"
      let current = new Date()
      let month1 = (current.getMonth() + 1) < 10 ? '0' + (current.getMonth() + 1) : current.getMonth() + 1;//获取当前月份
      let day1 = current.getDate() < 10 ? '0' + current.getDate() : current.getDate();//获取当前日份
      data.currentclasstime.dateandtime = month1 + "-" + day1
      let date = new Date();//获取当前时间
      date.setMonth(9 - 1)//设置当前月份为9月份
      for (let i = 1; i <= 7; i++) {
        date.setDate(i)//设置第九月份第一天
        let year = date.getFullYear()//获取当前年份
        let month = (date.getMonth() + 1) < 10 ? '0' + (date.getMonth() + 1) : date.getMonth() + 1;//获取当前月份
        let day = date.getDate() < 10 ? '0' + date.getDate() : date.getDate();//获取当前日份
        if (getweekday(year + "-" + month + "-" + day) === "星期一")//如果当前日期是星期一
        {
          data.currentclasstime.data = year + "-" + month + "-" + day
          for (let j = 0; j <= 6; j++)//遍历0到六  实际就是星期一到星期六
          {
            if (j !==0)//当前是星期一 日份不用加1
              date.setDate(date.getDate() + 1)//日份加一 直到星期日为结束
            year = date.getFullYear()
            month = (date.getMonth() + 1) < 10 ? '0' + (date.getMonth() + 1) : date.getMonth() + 1;
            day = date.getDate() < 10 ? '0' + date.getDate() : date.getDate();
            console.log("日份为", date.getDate())
            data.dateandtime[0][j + 1]['message'] = month + "-" + day
            if (j === 6) {
              data.currentclasstime.date = date
              data.currentclasstime.date.setDate(data.currentclasstime.date.getDate() + 1)
              data.currentclasstime.data += ~year + "-" + month + "-" + day
            }
          }
          break;

        }
      }

      /*const  time=new Data*/
      data.showdataevisiable = true;
      data.message=  "Current Week: Week"+data.currentclasstime.time+","+data.currentclasstime.data
    }

// 计算指定时间是星期几
    function getweekday(date){
      // date例如:'2022-03-05'
      var weekArray = new Array("星期日","星期一", "星期二", "星期三", "星期四", "星期五", "星期六")
      var week  = weekArray[new Date(date).getDay()]
      return week
    }


    const thisweekfuntion = () => {

      while(data.currentclasstime.time!=1)//回到第一周
      {
        lastweekfuntion()
      }
      data.currentclasstime.date.setDate(data.currentclasstime.date.getDate()-7)
      console.log("当前日s为",data.currentclasstime.date.getDate())
      data.currentclasstime.time=0
      let current=new Date()
      let fortime=new Date()
      while(data.currentclasstime.time!=16)
      {
        fortime.setMonth(data.currentclasstime.date.getMonth())
        fortime.setDate(data.currentclasstime.date.getDate())
        console.log("当前开始日",fortime.getDate())
        for(let j=0;j<=6;j++) {
          if (j !==0)
            fortime.setDate(fortime.getDate() + 1)
          if (fortime.getDate() === current.getDate() && fortime.getMonth() === current.getMonth())
          {
            nextweekfuntion()
            return
          }

        }
        nextweekfuntion()
      }
    }

    const lastweekfuntion=()=>{
      if(data.currentclasstime.time===1)
      {
        return
      }
      let date=new Date()
      date.setMonth(data.currentclasstime.date.getMonth())
      date.setDate(data.currentclasstime.date.getDate())
      date.setDate(date.getDate()-14)
      console.log("日份为",date.getDate())
      let year=date.getFullYear()//获取当前年份
      let month = (date.getMonth()+1)<10 ? '0'+(date.getMonth()+1) : date.getMonth()+1;//获取当前月份
      let day = date.getDate()<10 ? '0'+date.getDate() : date.getDate();//获取当前日份
      data.currentclasstime.data=year+"-"+month+"-"+day
      for(let j=0;j<=6;j++)//遍历0到六  实际就是星期一到星期六
      {
        if(j!==0)//当前是星期一 日份不用加1
          date.setDate(date.getDate()+1)//日份加一 直到星期日为结束  date改变会引起data.currentclasstime.date改变
        year=date.getFullYear()
        month = (date.getMonth()+1)<10 ? '0'+(date.getMonth()+1) : date.getMonth()+1;
        day = date.getDate()<10 ? '0'+date.getDate() : date.getDate();
        data.dateandtime[0][j+1]['message'] = month+"-"+day
        if(j===6)
        {
          data.currentclasstime.date=date
          data.currentclasstime.date.setDate(data.currentclasstime.date.getDate()+1)
          data.currentclasstime.data+=~year+"-"+month+"-"+day
        }
      }
      data.currentclasstime.time-=1
      data.message=  "Current Week: Week"+data.currentclasstime.time+","+data.currentclasstime.data
    }

    const nextweekfuntion=()=>
    {
      if(data.currentclasstime.time===16)
      {
        return
      }
      let date=new Date()
      date.setMonth(data.currentclasstime.date.getMonth())
      date.setDate(data.currentclasstime.date.getDate())
      console.log("日份为",date.getDate())
      let year=date.getFullYear()//获取当前年份
      let month = (date.getMonth()+1)<10 ? '0'+(date.getMonth()+1) : date.getMonth()+1;//获取当前月份
      let day = date.getDate()<10 ? '0'+date.getDate() : date.getDate();//获取当前日份
      data.currentclasstime.date=date
      data.currentclasstime.data=year+"-"+month+"-"+day
      for(let j=0;j<=6;j++)//遍历0到六  实际就是星期一到星期六
      {
        if(j!==0)//当前是星期一 日份不用加1
          date.setDate(date.getDate()+1)//日份加一 直到星期日为结束
        year=date.getFullYear()
        month = (date.getMonth()+1)<10 ? '0'+(date.getMonth()+1) : date.getMonth()+1;
        day = date.getDate()<10 ? '0'+date.getDate() : date.getDate();
        data.dateandtime[0][j+1]['message'] = month+"-"+day
        if(j===6)
        {
          data.currentclasstime.date=date
          data.currentclasstime.date.setDate(data.currentclasstime.date.getDate()+1)
          data.currentclasstime.data+=~year+"-"+month+"-"+day
        }
      }
      data.currentclasstime.time+=1
      data.message=  "Current Week: Week"+data.currentclasstime.time+","+data.currentclasstime.data

    }



    return{
      thisweekfuntion,
      lastweekfuntion,
      showdate,
      nextweekfuntion,
      ...toRefs(data),
    }
    //
  }
}



</script>