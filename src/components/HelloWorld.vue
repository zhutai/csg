<template>
    <el-row>
      <el-col :span="8">
        <el-card class="box-card" :body-style="{'padding':'0px'}">
          <div class="clearfix">
            业务查询条件
          </div>
          <el-form ref="form" :model="form" label-width="80px">
              <el-form-item label="业务名称">
                <el-input size="mini" v-model="form.name"></el-input>
              </el-form-item>
              <el-form-item label="速率">
                <el-col :span="9">
                  <el-select size="mini" v-model="form.region">
                    <el-option label="区域一" value="shanghai"></el-option>
                    <el-option label="区域二" value="beijing"></el-option>
                  </el-select>
                </el-col>
                <el-col  class="text-label" :span="6">业务类型</el-col>
                <el-col :span="9">
                    <el-select size="mini" v-model="form.type">
                      <el-option label="业务类型1" value="shanghai"></el-option>
                      <el-option label="业务类型2" value="beijing"></el-option>
                    </el-select>
                </el-col>
              </el-form-item>
              <el-form-item label="A站端点">
                <el-col :span="9">                
                  <el-input size="mini" v-model="form.aEndpoint"></el-input>
                </el-col>
                <el-col class="text-label" :span="6">Z站端点</el-col>                
                <el-col :span="9">
                  <el-input size="mini" v-model="form.zEndpoint"></el-input>                  
                </el-col>
              </el-form-item>
              <el-form-item>
                <el-col :span="16">                
                </el-col>
                <el-col :span="8">     
                  <el-button  size="mini" type="primary" @click="submitForm()">立即查询</el-button>           
                </el-col>
              </el-form-item>
          </el-form>
          <div class="clearfix">
            业务列表
          </div>
          <div class="table-list">
            <el-table size="mini"
              :data="tableSelfData"
              border
              @row-click="tableClick"
              style="width: 100%">
              <el-table-column
                prop="index"
                label="序号"
                width="150">
              </el-table-column>
              <el-table-column
                prop="name"
                label="电路名称">
              </el-table-column>
              <el-table-column
                prop="type"
                label="业务类型"
                width="120">
              </el-table-column>
            </el-table>
            <el-pagination v-if="tableSelfData"
              background @current-change="pageChange"
              layout="prev, pager, next"
              :total="tableData.length">
            </el-pagination>
          </div>
        </el-card>
      </el-col>
      <el-col :span="16" style="padding-left:20px;">
        <div class="div-iframe">
          <div style="width:100%;">
            <div class="clearfix">
              电路路由图
            </div>
            <div class="start-iframe">
              <iframe id="startIframe" v-if="startIframeUrl" :src="startIframeUrl"></iframe>
              <p class="text-iframe" v-else>没有数据，请先选择左侧表格点进行图表展示</p>              
            </div>
          </div>
          <div style="width:100%;">
            <div class="clearfix">
              光路图
            </div>
            <div class="end-iframe">
              <iframe id="endIframe" v-if="endIframeUrl" :src="endIframeUrl"></iframe>
              <p class="text-iframe" v-else>{{textStr}}</p>
            </div> 
          </div>
        </div>
      </el-col>
    </el-row>
</template>

<script>
export default {
  name: "HelloWorld",
  data() {
    return {
      form: {},
      startIframeUrl: "",
      endIframeUrl: "",
      textStr: "没有数据，请先选择左侧表格点进行图表展示",
      tableData: null,
      tableSelfData: null,
      tableDataArr: null
    };
  },
  created: function() {
    this.message();
  },
  methods: {
    submitForm: function() {
      this.tableData = [
        {
          index: "100000000000502",
          type: "继电保护1",
          name: "上海市普陀区金沙江路1518"
        },
        {
          index: "100000000000503",
          type: "继电保护2",
          name: "上海市普陀区金沙江路1518"
        },
        {
          index: "100000000000504",
          type: "继电保护3",
          name: "上海市普陀区金沙江路1518"
        },
        {
          index: "100000000000505",
          type: "继电保护4",
          name: "上海市普陀区金沙江路1518"
        },
        {
          index: "100000000000506",
          type: "继电保护5",
          name: "上海市普陀区金沙江路1518"
        },
        {
          index: "100000000000507",
          type: "继电保护6",
          name: "上海市普陀区金沙江路1518"
        },
        {
          index: "100000000000508",
          type: "继电保护7",
          name: "上海市普陀区金沙江路1518"
        },
        {
          index: "100000000000509",
          type: "继电保护8",
          name: "上海市普陀区金沙江路1518"
        },
        {
          index: "100000000000510",
          type: "继电保护9",
          name: "上海市普陀区金沙江路1518"
        },
        {
          index: "100000000000511",
          type: "继电保护10",
          name: "上海市普陀区金沙江路1518"
        },
        {
          index: "100000000000512",
          type: "继电保护11",
          name: "上海市普陀区金沙江路1518"
        },
        {
          index: "100000000000513",
          type: "继电保护12",
          name: "上海市普陀区金沙江路1518"
        },
      ];
      let arr = this.sliceArray(this.tableData, 10);
      this.tableDataArr = arr;
      this.tableSelfData = arr[0];
    },
    pageChange: function(index) {
      this.tableSelfData = this.tableDataArr[index - 1];
    },
    message: function() {
      let self = this;
      window.addEventListener("message", function(obj) {
        let data;
        if (typeof obj.data === "string") {
          data = JSON.parse(obj.data);
        } else {
          data = obj.data;
        }

        if (data.source === "EdgeClick" && self.startIframeUrl) {
          self.endIframeUrl = "http://www.runoob.com";
          // document.getElementById("endIframe").contentWindow.location.href = self.endIframeUrl;
          console.log(obj);
        } else if (data.source === "NoneClick") {
          self.endIframeUrl = "http://www.baidu.com";
        }
      });
    },
    tableClick: function(item) {
      console.log(item);
      let url = "http://192.168.11.211/tarsier-vmdb/diagram/index.html?code=0401#/tarsier/scenes/diagramDetail/"+item.index;
      this.startIframeUrl = url
      this.textStr = "请点击上图线条进行详细查看";
      this.endIframeUrl = "";
    },
    sliceArray: function(array, size) {
      var result = [];
      for (var x = 0; x < Math.ceil(array.length / size); x++) {
        var start = x * size;
        var end = start + size;
        result.push(array.slice(start, end));
      }
      return result;
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.el-row {
  padding: 20px;
}

.box-card {
  width: 100%;
}
.clearfix {
  padding: 12px 0;
  border-bottom: 1px solid #ddd;
  background: #d1deea;
  color: #409eff;
  font-size: 15px;
  text-align: center;
}
.el-form-item {
  margin-bottom: 0px;
}
.el-card__header {
  background: #d3dce6;
  padding: 15px;
}
.el-form {
  padding: 12px;
}
.el-card__body {
  padding: 0px;
}
.text-label {
  padding-right: 10px;
  text-align: right;
  color: #606266;
}
.table-list {
  padding: 15px;
  min-height: 420px;
}

.el-table th,
.el-table td {
  padding: 6px 0;
}

.div-iframe {
  width: 100%;
  border-radius: 5px;
  box-shadow: 0 2px 12px 0 rgba(0, 0, 0, 0.1);
}

.start-iframe {
  width: 100%;
  min-height: 320px;
}
.end-iframe {
  width: 100%;
  min-height: 320px;
}
iframe {
  width: 100%;
  min-height: 320px;
  border-width: 0px;
}

.text-iframe {
  text-align: center;
  font-size: 24px;
  padding-top: 100px;
}

.el-pagination {
  text-align: center;
  padding-top: 15px;
}
</style>
