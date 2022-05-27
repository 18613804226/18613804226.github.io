---
title: VUE+ELEMENT-UI的后台项目封装组件--TABLE的封装
top: false
cover: false
toc: true
mathjax: true
date: 2021-12-22 15:53:50
password:
summary:
tags: vue element
categories: 封装UI组件
---
子组件
```
<!--
 * @Descripttion: 
 * @version: 
 * @Author: sueRimn
 * @Date: 2020-06-18 17:40:26
 * @LastEditors: sueRimn
 * @LastEditTime: 2020-06-19 15:24:39
-->
<template>
  <div class="table">
    <el-table
      :data="tableData"
      style="width: 100%"
      border
      min-width="1190"
      max-height="300"
      :header-row-class-name="tableRowClassName"
      @selection-change="handleSelectionChange"
      @row-click="clickTable"
    >
      <template v-for="(item, index) of columns">
        <el-table-column
          v-if="item.id === 'text'"
          :key="index"
          :fixed="item.fixed"
          :prop="item.id"
          :label="item.label"
          :align="item.align ? item.align : 'center'"
          :width="item.width"
        >
          <!--if判断的是父组件传的表头是操作的id名-->
          <template slot-scope="scope">
            <el-button
              v-for="item1 in item.list"
              :key="item1.id"
              @click="handleDelete(scope.row, item1.id)"
              type="text"
              size="small"
              >{{ item1.name }}</el-button
            >
            <!--可以自行增加按钮，请改变点击事件的第二个参数，父组件会根据第二个参数判断当前点击的是什么按钮-->
          </template>
        </el-table-column>
        <el-table-column
          v-else-if="item.id === 'button'"
          :key="index"
          :fixed="item.fixed"
          :prop="item.id"
          :label="item.label"
          :align="item.align ? item.align : 'center'"
          :width="item.width"
        >
          <!--if判断的是父组件传的表头是操作的id名-->
          <template slot-scope="scope">
            <el-button
              v-for="item2 in item.list"
              :key="item2.id"
              @click="handleEdit(scope.row, item2.id)"
              size="mini"
              :type="item2.type"
              >{{ item2.name }}</el-button
            >
            <!--可以自行增加按钮，请改变点击事件的第二个参数，父组件会根据第二个参数判断当前点击的是什么按钮-->
          </template>
        </el-table-column>
        <el-table-column
          v-else-if="item.index === 'index'"
          :type="item.index"
          :key="index"
          :width="item.width"
        >
        </el-table-column>
        <el-table-column
          v-else
          :label="item.label"
          :key="index"
          :fixed="item.fixed"
          :prop="item.id"
          :align="item.align ? item.align : 'center'"
          :width="item.width"
          :type="item.type"
        >
        </el-table-column>
        <!--可以传align,width和type来控制表格的居中，宽度和类型（比如需要序号，type传index）-->
      </template>
    </el-table>
    <el-pagination
      @size-change="handleSizeChange"
      @current-change="handleCurrentChange"
      :current-page="currentPage"
      :page-sizes="pagesizes"
      :page-size="pageSize"
      layout="total, sizes, prev, pager, next, jumper"
      :total="total"
    >
    </el-pagination>
  </div>
</template>
<script>
export default {
  props: {
    tableData: {
      // 表格数据源 默认为空数组
      type: Array,
      default: () => []
    },
    columns: {
      // 表格的字段展示 默认为空数组
      type: Array,
      default: () => []
    },
    pagesizes: {
      type: Array,
      default: () => []
    },
    total: { type: Number, default: 0 },
    pageSize: { type: Number, default: 0 }
  },
  data() {
    return {
      currentPage: 1
    };
  },
  methods: {
    // 正常
    handleSelectionChange(val) {
      this.$emit("handleSelectionChange", { val: val });
    },
    // 正常
    clickTable(row, column, event) {
      this.$emit("clickTable", { row: row, column: column, event: event });
    },
    // 正常
    handleEdit(index, row) {
      this.$emit("handleEdit", { index: index, row: row });
    },
    // 正常
    handleDelete(index, row) {
      this.$emit("handleDelete", { index: index, row: row });
    },
    handleSizeChange(val) {
      console.log(`每页 ${val} 条`);
      this.$emit("handleSizeChange", val);
    },
    handleCurrentChange(val) {
      console.log(`当前页: ${val}`);
      this.$emit("handleCurrentChange", val);
    },
    tableRowClassName({ row, rowIndex }) {
      console.log(row, rowIndex);
      if (rowIndex === 0) {
        return "warning-row";
      } else if (rowIndex === 1) {
        return "warning-row";
      }
      return "";
    }
  }
};
</script>
<style lang="less">
.el-pagination {
  margin-top: 20px;
}
//.warning-row .is-leaf 是修改table的表头的背景颜色；
.warning-row .is-leaf { color: #1a68b5; background-color: #e3edf7 !important; } </style>
```

父组件
```
1 <flight-table
  2           :tableData="tableData"
  3           :columns="columns"
  4           :pagesizes="pagesizes"
  5           @handleDelete="handleDelete"
  6           @handleSelectionChange="handleSelectionChange"
  7           @clickTable="clickTable"
  8           @handleEdit="handleEdit"
  9           @handleSizeChange="handleSizeChange"
 10           @handleCurrentChange="handleCurrentChange"
 11           :total="total"
 12           :pageSize="pageSize"
 13 ></flight-table>
 14 import flightTable from "@/components/flightTable.vue";    import jsondata from "assets/json/vue.json";15  export default{
 16   components: {
 17     flightTable ,
 18  19   },
 20     data() {
 21         return {
 22            tableData: [
 23         {
 24           date: "2016-05-03",
 25           name: "王小虎",
 26           province: "上海",
 27           city: "普陀区",
 28           address: "上海市普陀区金沙江路 1518 弄",
 29           zip: 200333
 30         },
 31         {
 32           date: "2016-05-02",
 33           name: "王小虎",
 34           province: "上海",
 35           city: "普陀区",
 36           address: "上海市普陀区金沙江路 1518 弄",
 37           zip: 200333
 38         },
 39         {
 40           date: "2016-05-04",
 41           name: "王小虎",
 42           province: "上海",
 43           city: "普陀区",
 44           address: "上海市普陀区金沙江路 1518 弄",
 45           zip: 200333
 46         },
 47         {
 48           date: "2016-05-01",
 49           name: "王小虎",
 50           province: "上海",
 51           city: "普陀区",
 52           address: "上海市普陀区金沙江路 1518 弄",
 53           zip: 200333
 54         },
 55         {
 56           date: "2016-05-08",
 57           name: "王小虎",
 58           province: "上海",
 59           city: "普陀区",
 60           address: "上海市普陀区金沙江路 1518 弄",
 61           zip: 200333
 62         },
 63         {
 64           date: "2016-05-06",
 65           name: "王小虎",
 66           province: "上海",
 67           city: "普陀区",
 68           address: "上海市普陀区金沙江路 1518 弄",
 69           zip: 200333
 70         },
 71         {
 72           date: "2016-05-07",
 73           name: "王小虎",
 74           province: "上海",
 75           city: "普陀区",
 76           address: "上海市普陀区金沙江路 1518 弄",
 77           zip: 200333
 78         }
 79       ],
 80       columns: [
 81         {
 82           id: "selection",
 83           type: "selection",
 84           label: "",
 85           fixed: "left",
 86           width: "55",
 87           prop: "",
 88           isShow: true,
 89           align: "center"
 90         },
 91         {
 92           id: "button",
 93           type: "button",
 94           label: "操作",
 95           fixed: "left",
 96           width: "200",
 97           prop: "",
 98           isShow: true,
 99           align: "center",
100           list: [
101             {
102               id: "examine",
103               name: "查看",
104               type: ""
105             },
106             {
107               id: "compile",
108               name: "编辑",
109               type: "danger"
110             }
111           ]
112         },
113         {
114           id: "text",
115           type: "text",
116           label: "跳转",
117           fixed: "left",
118           width: "120",
119           prop: "",
120           isShow: true,
121           align: "center",
122           list: [{ id: "jump", name: "跳转", type: "", handleClick: this.jump }]
123         },
124         {
125           id: "index",
126           type: "index",
127           label: "序列号",
128           fixed: "left",
129           width: "120",
130           prop: "",
131           isShow: true,
132           align: "center"
133         },
134         {
135           id: "date",
136           type: "",
137           label: "日期",
138           fixed: "left",
139           width: "150",
140           prop: "date",
141           isShow: true,
142           align: "center"
143         },
144         {
145           id: "name",
146           type: "",
147           label: "姓名",
148           fixed: false,
149           width: "120",
150           prop: "name",
151           isShow: true,
152           align: "center"
153         },
154         {
155           id: "province",
156           type: "",
157           label: "省份",
158           fixed: false,
159           width: "120",
160           prop: "province",
161           isShow: true,
162           align: "center"
163         },
164         {
165           id: "city",
166           type: "",
167           label: "市区",
168           fixed: false,
169           width: "120",
170           prop: "city",
171           isShow: true,
172           align: "center"
173         },
174         {
175           id: "address",
176           type: "",
177           label: "地址",
178           fixed: false,
179           width: "300",
180           prop: "address",
181           isShow: true,
182           align: "center"
183         },
184         {
185           id: "zip",
186           type: "",
187           label: "邮编",
188           fixed: false,
189           width: "120",
190           prop: "zip",
191           isShow: true,
192           align: "center"
193         }
194       ],
195       pagesizes: [1, 2, 3, 4],
196       total: 10,
197       pageSize: 1
198          }
199     },
200     methods: {
201       //text的跳转触发
202     handleDelete(val) {
203       console.log(val);
204       this.StringUtil.getDecorator();
205     },
206     // checkbox的触发接收
207     handleSelectionChange(val) {
208       console.log(val);
209       this.StringUtil.getDecorator();
210     },
211     // 点击tbody的行触发
212     clickTable(val) {
213       console.log(val);
214       this.StringUtil.getDecorator();
215     },
216     //button的触发
217     handleEdit(val) {
218       console.log(val);
219       this.StringUtil.getDecorator();
220     },
221     handleSizeChange(val) {
222       console.log(val);
223       this.total = 100;
224       this.tableData = jsondata.tabledata;
225       console.log(jsondata.tableData);
226     },
227     handleCurrentChange(val) {
228       console.log(val);
229       this.total = 200;
230 
231       this.tableData = jsondata.tabledata2;
232     }
233     }
234 }
235  </script>
```
