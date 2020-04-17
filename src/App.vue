<template>
  <div id="app">
    <input-click
      :dataList="dataList"
      :keyBody="keyBody"
      :bodyRow="bodyRow"
      @addRow="addRow"
      @delRow="delRow"
      @addCol="addCol"
      @delCol="delCol"
    />
    <!-- 提交表格数据 -->
    <div style="textAlign:center">
      <button @click="submit">submit table</button>
    </div>
  </div>
</template>

<script>
import InputClick from "./components/InputClick";
import { submitTableData } from "@/api/apis";
export default {
  components: {
    InputClick
  },
  data() {
    return {
      //   表格初始数据
      dataList: {
        tableHead: ["name", "money", "deat"],
        tableBody: [
          { body1: "arron", body2: 1890.00, body3: "132123.00" },
          { body1: "allen", body2: 198.00, body3: "12312.00" },
          { body1: "betty", body2: 2100.00, body3: "123123.00" },
          { body1: "rose", body2: 2200.00, body3: "123123.00" },
          { body1: "adem", body2: 2300.00, body3: "131231.00" }
        ]
      },
      //tableBody对象key值数组
      keyBody: ["body1", "body2", "body3"],
      // 初始表格行
      bodyRow: {
        body1: "",
        body2: "",
        body3: ""
      }
    };
  },
  methods: {
    // 添加行
    addRow(index) {
      this.dataList.tableBody.splice(
        index + 1,
        0,
        JSON.parse(JSON.stringify(this.bodyRow))
      );
    },
    // 删除行
    delRow(index) {
      this.dataList.tableBody.splice(index, 1);
    },
    // 添加列
    addCol(length) {
      let key = "body" + length;
      this.keyBody.push(key);
      this.dataList.tableHead.splice(length, 0, "");
      this.dataList.tableBody.forEach((item, index) => {
        this.dataList.tableBody[index] = {
          ...this.dataList.tableBody[index],
          [key]: ""
        };
      });
      this.bodyRow = { ...this.bodyRow, [key]: "" };
    },
    // 删除列
    delCol() {
      let length = this.dataList.tableHead.length;
      let keyBodyLength = this.keyBody.length - 1;
      this.dataList.tableHead.splice(length - 1, 1);
      this.dataList.tableBody.forEach((item, index) => {
        delete item[this.keyBody[keyBodyLength]];
      });
      this.keyBody.pop();
    },
    // 提交表格数据
    submit() {
      let params = {
        tableHead: JSON.stringify({ ...this.dataList.tableHead }),
        tableBody: JSON.stringify({ ...this.dataList.tableBody })
      };
      submitTableData(params)
        .then(res => {
          let { code, data } = res;
          if (code === 0) {
            console.log(提交成功);
          }
        })
        .catch(err => {
          console.log(err);
        });
    }
  }
};
</script>

<style lang="less" scoped>
</style>