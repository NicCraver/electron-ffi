<template>
  <div>
    <div class="button" @click="test">测 试</div>
  </div>
</template>

<script>
import SystemInformation from "./LandingPage/SystemInformation";
const ffi = require("ffi");
const path = require("path");
const iconvLite = require('iconv-lite');
export default {
  name: "landing-page",
  components: { SystemInformation },
  methods: {
    test() {
      console.log(iconvLite)
      //首先在components下创建一个文件夹dll,将dll文件放在dll文件夹中
      var libpath = path.join(__dirname, "../../../static/Sdtapi.dll");
      // var libpath = path.join(__static, "../../../static/dll/Sdtapi.dll");
      // var libpath = path.join(__static, "/static/Sdtapi.dll");
      // var libpath = path.join(__static, "/Sdtapi.dll");
      console.log("__dirname--------"+__dirname)
      console.log("__static--------"+__static)
      console.log("使用路径--------"+libpath);
      
      var testLib = ffi.Library(libpath, {
        InitComm: ["int32", ["int"]],
        //方法名      方法类型   参数类型
        Authenticate: ["int32", ["void"]],
        ReadBaseInfos: [
          "int",
          [
            "string",
            "string",
            "string",
            "string",
            "string",
            "string",
            "string",
            "string",
            "string"
          ]
        ]
      });
      var a = testLib.InitComm(1001);
      console.log("端口初始化函数---", a); //输出1为成功
      var b = testLib.Authenticate(null);
      console.log("卡认证接口---", b);

      var Name = new Buffer(31);
      var Gender = new Buffer(10);
      var Folk = new Buffer(9);
      var BirthDay = new Buffer(71);
      var Code = new Buffer(19);
      var Address = new Buffer(31);
      var Agency = new Buffer(9);
      var ExpireStart = new Buffer(9);
      var ExpireEnd = new Buffer(9);
      //打印一下
      console.log(
        Name,
        Gender,
        Folk,
        BirthDay,
        Code,
        Address,
        Agency,
        ExpireStart,
        ExpireEnd
      );

      var c = testLib.ReadBaseInfos(
        Name,
        Gender,
        Folk,
        BirthDay,
        Code,
        Address,
        Agency,
        ExpireStart,
        ExpireEnd
      );
      console.log("身份证信息---", c);
      console.log(
        Name,
        Gender,
        Folk,
        BirthDay,
        Code,
        Address,
        Agency,
        ExpireStart,
        ExpireEnd
      );
      // console.log(msg.name)
      // // buf.toString([encoding],[start],[end]);
      // // console.log( Buffer.isEncoding('utf-8'))
      // // let str = decoder.write(Name);
      // let str = decoder.end(Name);
      // console.log(str);  // 好
      // console.log(str.toString());
      console.log("姓名",Name.toString());
      // console.log(Gender.toString());
      // console.log(Folk.toString());
      // console.log(BirthDay.toString());
      console.log(Code.toString());
      // console.log(Address.toString());
      // console.log(Agency.toString());
      // console.log(ExpireStart.toString());
      // console.log(ExpireEnd.toString());
      // var tname = Name.toString()
      var decodedBody = iconvLite.decode(Name, 'gbk');
      // var lname = iconvLite.decode(tname,'gbk');
      console.log("姓名",decodedBody)
    }
  }
};
</script>

<style>
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}
.button {
  font-family: "Helvetica Neue", Helvetica, "PingFang SC", "Hiragino Sans GB",
    "Microsoft YaHei", "微软雅黑", Arial, sans-serif;
  margin: 200px auto;
  width: 200px;
  line-height: 50px;
  text-align: center;
  background: #409eff;
  color: #ffffff;
  font-size: 18px;
  border-radius: 4px;
}
</style>
