<template>
  <div class="pos">
    <el-row>
      <el-col :span="7" class="pos-order" id="order-list">
        <el-tabs>
          <el-tab-pane label="点餐">
            <el-table :data="tableData" border style="width:100%">
              <el-table-column prop="goodsName" label="商品名称"></el-table-column>
              <el-table-column prop="count" label="数量"></el-table-column>
              <el-table-column prop="price" label="金额"></el-table-column>
              <el-table-column label="操作" fixed="right">
                <template slot-scope="scope">
                  <el-button type="text" size="small" @click="delSingleGoods(scope.row)">删除</el-button>
                  <el-button type="text" size="small" @click="addOrderList(scope.row)">增加</el-button>
                </template>
              </el-table-column>
            </el-table>

            <div class="totalDiv">
              <small>数量:</small>
              {{totalCount}} &nbsp;&nbsp;&nbsp;&nbsp;
              <small>金额:</small>
              {{totalMoney}}元
            </div>

            <div class="div-btn">
              <el-button type="warning">挂单</el-button>
              <el-button type="danger" @click="delAllGoods()">删除</el-button>
              <el-button type="success" @click="checkout()">结账</el-button>
            </div>
          </el-tab-pane>
          <el-tab-pane label="挂单">挂单</el-tab-pane>
          <el-tab-pane label="外卖">外卖</el-tab-pane>
        </el-tabs>
      </el-col>
      <el-col span="17">
        <div class="often-goods">
          <div class="title">常用商品</div>
          <div class="often-goods-list">
            <ul>
              <li v-for="(goods,index) in oftenGoods" :key="index" @click="addOrderList(goods)">
                <span>{{goods.goodsName}}</span>
                <span class="o-price">¥{{goods.price}}元</span>
              </li>
            </ul>
          </div>
        </div>

        <div class="goods-type">
          <el-tabs>
            <el-tab-pane label="汉堡">
              <div>
                <ul class="cookList">
                  <li v-for="(goods,index) in type0Goods" :key="index" @click="addOrderList(goods)">
                    <span class="foodImg">
                      <img :src="goods.goodsImg" width="100%" />
                    </span>
                    <span class="foodName">{{goods.goodsName}}</span>
                    <span class="foodPrice">￥{{goods.price}}元</span>
                  </li>
                </ul>
              </div>
            </el-tab-pane>
            <el-tab-pane label="小食">
              <div>
                <ul class="cookList">
                  <li v-for="(goods,index) in type1Goods" :key="index" @click="addOrderList(goods)">
                    <span class="foodImg">
                      <img :src="goods.goodsImg" width="100%" />
                    </span>
                    <span class="foodName">{{goods.goodsName}}</span>
                    <span class="foodPrice">￥{{goods.price}}元</span>
                  </li>
                </ul>
              </div>
            </el-tab-pane>
            <el-tab-pane label="饮料">
              <div>
                <ul class="cookList">
                  <li v-for="(goods,index) in type2Goods" :key="index" @click="addOrderList(goods)">
                    <span class="foodImg">
                      <img :src="goods.goodsImg" width="100%" />
                    </span>
                    <span class="foodName">{{goods.goodsName}}</span>
                    <span class="foodPrice">￥{{goods.price}}元</span>
                  </li>
                </ul>
              </div>
            </el-tab-pane>
            <el-tab-pane label="套餐">
              <div>
                <ul class="cookList">
                  <li v-for="(goods,index) in type3Goods" :key="index" @click="addOrderList(goods)">
                    <span class="foodImg">
                      <img :src="goods.goodsImg" width="100%" />
                    </span>
                    <span class="foodName">{{goods.goodsName}}</span>
                    <span class="foodPrice">￥{{goods.price}}元</span>
                  </li>
                </ul>
              </div>
            </el-tab-pane>
          </el-tabs>
        </div>
      </el-col>
    </el-row>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "pos",
  data() {
    return {
      tableData: [
        // {
        //   goodsName: '可口可乐',
        //   price: 8,
        //   count:1
        // }, {
        //   goodsName: '香辣鸡腿堡',
        //   price: 15,
        //   count:1
        // }, {
        //   goodsName: '爱心薯条',
        //   price: 8,
        //   count:1
        // }, {
        //   goodsName: '甜筒',
        //   price: 8,
        //   count:1
        // }
      ],
      oftenGoods: [
        //   {
        //       goodsId:1,
        //       goodsName:'香辣鸡腿堡',
        //       price:18
        //   }, {
        //       goodsId:2,
        //       goodsName:'田园鸡腿堡',
        //       price:15
        //   }, {
        //       goodsId:3,
        //       goodsName:'和风汉堡',
        //       price:15
        //   }, {
        //       goodsId:4,
        //       goodsName:'快乐全家桶',
        //       price:80
        //   }, {
        //       goodsId:5,
        //       goodsName:'脆皮炸鸡腿',
        //       price:10
        //   }, {
        //       goodsId:6,
        //       goodsName:'魔法鸡块',
        //       price:20
        //   }, {
        //       goodsId:7,
        //       goodsName:'可乐大杯',
        //       price:10
        //   }, {
        //       goodsId:8,
        //       goodsName:'雪顶咖啡',
        //       price:18
        //   }, {
        //       goodsId:9,
        //       goodsName:'大块鸡米花',
        //       price:15
        //   }, {
        //       goodsId:20,
        //       goodsName:'香脆鸡柳',
        //       price:17
        //   }
      ],
      type0Goods: [
        // {
        //     goodsId:1,
        //     goodsImg:"https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1577812586842&di=2e6fed46408c5764c8d7982abb7e1ac5&imgtype=0&src=http%3A%2F%2Fe.hiphotos.baidu.com%2Fbainuo%2Fcrop%3D0%2C0%2C690%2C657%3Bw%3D690%3Bq%3D99%3Bc%3Dnuomi%2C95%2C95%2Fsign%3D456eaea6eddde711f39d19b69adfe22b%2F3ac79f3df8dcd1002aade6827a8b4710b9122f5b.jpg",
        //     goodsName:'香辣鸡腿堡',
        //     price:18
        // }, {
        //     goodsId:2,
        //     goodsImg:"https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1577812776219&di=f5860dfd307abae61071f91eb3270a61&imgtype=0&src=http%3A%2F%2Fwww.zhomei.com%2Fattachs%2F2016%2F05%2F07%2Fthumb_572dbe024b8a1.jpg",
        //     goodsName:'田园鸡腿堡',
        //     price:15
        // }, {
        //     goodsId:3,
        //     goodsImg:"https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1577813344530&di=8a282de9f4c99eaae5167bba2f12b952&imgtype=0&src=http%3A%2F%2Fwxt.sinaimg.cn%2Fthumb300%2F007kHh7igy1g05aunqxzsj30b409wgm0.jpg%3Ftags%3D%255B%255D",
        //     goodsName:'和风汉堡',
        //     price:15
        // }, {
        //     goodsId:4,
        //      goodsImg:"https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1577812873163&di=9276335638f3694720d92759c4bdae4c&imgtype=0&src=http%3A%2F%2Fwww.baibailai.com%2Fdata%2Ffiles%2Fstore_1137%2Fgoods_98%2Fsmall_201307261531382755.jpg",
        //     goodsName:'快乐全家桶',
        //     price:80
        // }, {
        //     goodsId:5,
        //      goodsImg:"https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1577812934149&di=741c050b49dfc859c2d2c4fb4bc7b7e5&imgtype=0&src=http%3A%2F%2Fimg7.21food.cn%2Fsimg%2Falbum%2F2019%2F12%2F10%2Fyipin8881436045.jpg",
        //     goodsName:'脆皮炸鸡腿',
        //     price:10
        // }, {
        //     goodsId:6,
        //      goodsImg:"https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1577812988905&di=98bf969215737713326cbb04dbc7093b&imgtype=0&src=http%3A%2F%2Fimgsrc.baidu.com%2Fforum%2Fw%3D580%2Fsign%3D71632dcec8ef76093c0b99971edca301%2F5fd14610b912c8fcbe59df9eff039245d7882190.jpg",
        //     goodsName:'魔法鸡块',
        //     price:20
        // }, {
        //     goodsId:7,
        //      goodsImg:"https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1577813075203&di=24664393d03eebf315061cde0f0e2f47&imgtype=0&src=http%3A%2F%2Fg-search1.alicdn.com%2Fimg%2Fbao%2Fuploaded%2Fi3%2F1664363837%2FO1CN011eDOAP4cBjRTBLG_%2521%25211664363837.jpg_300x300.jpg",
        //     goodsName:'可乐大杯',
        //     price:10
        // }, {
        //     goodsId:8,
        //      goodsImg:"https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1577813184173&di=930968f070716d36b6bbe72034ab6006&imgtype=0&src=http%3A%2F%2Fg-search1.alicdn.com%2Fimg%2Fbao%2Fuploaded%2Fi1%2F2486797410%2FTB2hKZntCtYBeNjSspaXXaOOFXa_%2521%25212486797410.jpg_300x300.jpg",
        //     goodsName:'雪顶咖啡',
        //     price:18
        // }, {
        //     goodsId:9,
        //      goodsImg:"https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1577813235782&di=d2160903d9f98046a36092e9d856291f&imgtype=jpg&src=http%3A%2F%2Fimg3.imgtn.bdimg.com%2Fit%2Fu%3D1671191393%2C179713699%26fm%3D214%26gp%3D0.jpg",
        //     goodsName:'大块鸡米花',
        //     price:15
        // }, {
        //     goodsId:20,
        //      goodsImg:"https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1577813296263&di=9d93d398db307853d2e41d7fd23fa2c1&imgtype=0&src=http%3A%2F%2Fimg3.doubanio.com%2Fview%2Fcommodity_story%2Fimedium%2Fpublic%2Fp9788651.jpg",
        //     goodsName:'香脆鸡柳',
        //     price:17
        // }
      ],
      type1Goods: [],
      type2Goods: [],
      type3Goods: [],
      totalMoney: 0,
      totalCount: 0
    };
  },
  created: function() {
    //拉取常用商品数据
    axios
      .get(
        "https://www.easy-mock.com/mock/5b8b30dbf032f03c5e71de7f/kuaican/oftenGoods"
      )
      .then(reponse => {
        console.log(reponse);
        this.oftenGoods = reponse.data;
      })
      .catch(error => {
        console.log(error);
        alert("网络错误，不能访问");
      });

    //拉取分类商品数据
    axios
      .get(
        "https://www.easy-mock.com/mock/5b8b30dbf032f03c5e71de7f/kuaican/typeGoods"
      )
      .then(reponse => {
        console.log(reponse);
        this.type0Goods = reponse.data[0];
        this.type1Goods = reponse.data[1];
        this.type2Goods = reponse.data[2];
        this.type3Goods = reponse.data[3];
      })
      .catch(error => {
        console.log(error);
        alert("网络错误，不能访问");
      });
  },
  mounted: function() {
    var orderHeiht = document.body.clientHeight;
    console.log(orderHeiht);
    document.getElementById("order-list").style.height = orderHeiht + "px";
  },
  methods: {
    //goods相当于一个商品对象
    addOrderList(goods) {
      //数量金额清零
      this.totalMoney = 0;
      this.totalCount = 0;

      //先判断商品是否已经存在于订单列表中
      let isHave = false;
      for (let i = 0; i < this.tableData.length; i++) {
        if (this.tableData[i].goodsId == goods.goodsId) {
          isHave = true;
        }
      }

      //根据判断的值编写业务逻辑
      if (isHave) {
        //改变列表中商品数量
        let arr = this.tableData.filter(o => o.goodsId == goods.goodsId);
        arr[0].count++;
      } else {
        let newGoods = {
          goodsId: goods.goodsId,
          goodsName: goods.goodsName,
          price: goods.price,
          count: 1
        };
        this.tableData.push(newGoods);
      }

      this.getAllMoney();

    },

    //删除单个商品
    delSingleGoods(goods) {
      this.tableData = this.tableData.filter(o => o.goodsId != goods.goodsId);
      this.getAllMoney();
    },
    //清除订单
    delAllGoods(){
      this.tableData=[];
      this.totalMoney=0;
      this.totalCount=0;
    },

    //模拟结账
    checkout(){
      if(this.totalCount!=0){
        this.tableData=[];
        this.totalCount=0;
        this.totalMoney=0;
        this.$message({
          message:'结账成功，感谢您为店里出了一份力!',
          type:'success'
        });
      }else{
        this.$message.error('不能空结，老板了解您急切的心情');
      }
    },

    //汇总数量和金额
    getAllMoney() {
      this.totalCount = 0;
      this.totalMoney = 0;
      if (this.tableData) {

        //计算汇总金额和数量
        this.tableData.forEach(element => {
          this.totalCount += element.count;
          this.totalMoney = this.totalMoney + element.price * element.count;
        });
      }
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.pos-order {
  background-color: #f9fafc;
  border-right: 1px solid #c0ccda;
}
.div-btn {
  margin-top: 10px;
}
.title {
  height: 20px;
  border-bottom: 1px solid #d3dce6;
  background-color: #f9fafc;
  padding: 10px;
  text-align: left;
}
.often-goods-list ul li {
  list-style: none;
  float: left;
  border: 1px solid #e5e9f2;
  padding: 10px;
  margin: 10px;
  background-color: #fff;
  cursor: pointer;
}
.o-price {
  color: #58b7ff;
}
.goods-type {
  clear: both;
}
.cookList li {
  list-style: none;
  width: 23%;
  border: 1px solid #e5e9f2;
  height: auot;
  overflow: hidden;
  background-color: #fff;
  padding: 2px;
  float: left;
  margin: 2px;
  cursor: pointer;
}
.cookList li span {
  display: block;
  float: left;
}
.foodImg {
  width: 40%;
}
.foodName {
  font-size: 18px;
  padding-left: 10px;
  color: brown;
  font-size: 16px;
}
.foodPrice {
  font-size: 16px;
  padding-left: 10px;
  padding-top: 10px;
}
.totalDiv {
  background-color: #fff;
  padding: 10px;
  border-bottom: 1px solid #d3dce6;
}
</style>
<style>
.el-tabs__nav-scroll {
  background-color: #fff;
}
</style>