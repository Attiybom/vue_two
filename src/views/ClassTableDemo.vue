<template>
  <div class="home">
    <div class="category">
      <div class="title">类目信息</div>
      <el-tabs v-model="activeName1" type="card">
        <!-- 第一层 -->
        <el-tab-pane label="服务类" name="server1">
          <div v-for="(item, index) in serviceList" :key="index">
            <span>{{ item.topName }}:</span>
            <el-tag v-for="children in item.Names" :key="children">
              {{ children }}
            </el-tag>
          </div>
        </el-tab-pane>
        <el-tab-pane label="商品类" name="goods1">
          <div v-for="(item, index) in goodsList" :key="index">
            <span>{{ item.topName }}:</span>
            <el-tag v-for="children in item.Names" :key="children">
              {{ children }}
            </el-tag>
          </div>
        </el-tab-pane>
        <!-- <template v-for="(category, index) in categoryRespList">
          <el-tab-pane
            :key="index"
            :name="category.type"
            :label="category.type"
          >
            <span v-for='children in category.topNames' :key='children'>{{ children.topName }}</span>
            <el-tag v-for="children in category.Names" :key="children">
              {{ children.Name }}
            </el-tag>
          </el-tab-pane>
        </template> -->
      </el-tabs>
    </div>
  </div>
</template>

<script>
import { getTabData } from "@/services/tabs";

export default {
  data() {
    return {
      activeName1: "server1",
      // categoryRespList: [
      //   {
      //     type: "service",
      //     topCode: "yi",
      //     topName: "医",
      //     Code: "ganmao",
      //     Name: "感冒",
      //   },
      //   {
      //     type: "service",
      //     topCode: "yi",
      //     topName: "医",
      //     Code: "fashao",
      //     Name: "发烧",
      //   },
      //   {
      //     type: "service",
      //     topCode: "zhu",
      //     topName: "住",
      //     Code: "ganmao",
      //     Name: "公寓",
      //   },
      //   {
      //     type: "goods",
      //     topCode: "chi",
      //     topName: "吃",
      //     Code: "fan",
      //     Name: "饭",
      //   },
      //   {
      //     type: "goods",
      //     topCode: "chi",
      //     topName: "吃",
      //     Code: "chi",
      //     Name: "面",
      //   },
      //   {
      //     type: "goods",
      //     topCode: "he",
      //     topName: "喝",
      //     categoryCode: "yinliao",
      //     Name: "饮料",
      //   },
      // ],
      categoryRespList: [],
    };
  },
  computed: {
    serviceList() {
      const temp = this.categoryRespList.filter(
        (item) => item.type === "service"
      );
      const groupedItems = {};

      temp.forEach((item) => {
        const { topCode, Name } = item;

        if (!groupedItems[topCode]) {
          groupedItems[topCode] = { ...item, Names: [Name] };
        } else {
          groupedItems[topCode].Names.push(Name);
        }
      });
      const mergedServiceList = Object.values(groupedItems);
      console.log('mergedServiceList', mergedServiceList)
      return mergedServiceList;
    },
    goodsList() {
      let temp = this.categoryRespList.filter((item) => {
        return item.type === "goods";
      });
      const groupedItems = {};

      temp.forEach((item) => {
        const { topCode, Name } = item;

        if (!groupedItems[topCode]) {
          groupedItems[topCode] = { ...item, Names: [Name] };
        } else {
          groupedItems[topCode].Names.push(Name);
        }
      });
      const mergedServiceList = Object.values(groupedItems);
      return mergedServiceList;
    },
  },
  mounted() {
    getTabData().then((res) => {
      // console.log('res', res)
      this.categoryRespList = res.tabData;
      // const obj = {};
      // this.categoryRespList.forEach((item) => {
      //   const { type, topName, Name, Code } = item;

      //   if (!obj[type]) {
      //     obj[type] = { ...item, Codes: [], Names: [], topNames: [] };
      //   }

      //   if (!obj[type].topNames.includes(topName)) {
      //     obj[type].topNames.push(topName);
      //   }

      //   if (!obj[type].Codes.includes(Code)) {
      //     obj[type].Codes.push(Code);
      //   }
      //   if (!obj[type].Names.includes(Name)) {
      //     obj[type].Names.push(Name);
      //   }
      // });

      // const objArr = Object.values(obj);
      // this.categoryRespList = objArr;
    });
  },
};
</script>
