<template>
  <div class="table">
    <!-- 数据内容 -->
    <el-table :data="currentData" style="width: 100%">
      <el-table-column prop="img" label="Hero Image" width="180">
        <template slot-scope="scope">
          <img :src="`https://api.opendota.com${scope.row.img}`" alt="" lazy />
        </template>
      </el-table-column>
      <el-table-column prop="name" sortable label="Hero Name" width="180">
      </el-table-column>
      <el-table-column prop="pro_win" label="PRO Win Rate" width="180">
      </el-table-column>
      <el-table-column prop="pro_pick" label="Pro Pick Count" width="180">
      </el-table-column>
      <el-table-column prop="pro_ban" label="Pro Ban Count" width="180">
      </el-table-column>
    </el-table>
    <!-- 分页 -->
    <el-pagination
      layout="prev, pager, next"
      :total="total"
      @current-change="currentChange"
      :page-size="pageSize"
    >
    </el-pagination>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "TableList",
  data() {
    return {
      tableData: [], //所有数据
      currentData: [], // 表格显示诗句
      currentPage: 1, // 当前页码
      total: 0, // 总条数
      pageSize: 5, // 每页数据
    };
  },
  props: {
    msg: String,
  },
  async mounted() {
    let res = await axios.get("https://api.opendota.com/api/heroStats");
    this.tableData = (res && res.data && res.data.slice(0, 10)) || [];
    this.currentData = this.tableData.slice(0, 5);
    this.total = this.tableData.length;
  },
  methods: {
    load() {},
    currentChange(val) {
      this.currentData = this.tableData.slice(
        (val - 1) * this.pageSize,
        val * this.pageSize
      );
      console.log(this.currentData);
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
img {
  width: 50px;
  height: 50px;
  border-radius: 50%;
}
</style>
