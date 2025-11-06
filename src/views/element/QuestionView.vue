<template>
  <el-container>
    <el-header style="font-size: 40px; background-color: rgb(238, 241, 246)"
      >Quiz后台管理</el-header
    >
    <el-container>
      <el-aside width="200px" style="background-color: rgb(238, 241, 246)">
        <el-menu :default-openeds="['1']">
          <el-submenu index="1">
            <template slot="title">管理选项</template>
            <el-menu-item-group>
              <el-menu-item index="1-1">
                <!-- 用户管理 -->
                <router-link to="/user">用户管理</router-link>
                </el-menu-item>
              <el-menu-item index="1-2">
                <!-- 题目管理 -->
                <router-link to="/question">题目管理</router-link>
                </el-menu-item>
            </el-menu-item-group>
          </el-submenu>

          <!-- <el-submenu index="2">
            <template slot="title"><i class="el-icon-menu"></i>导航二</template>
            <el-menu-item-group>
              <template slot="title">分组一</template>
              <el-menu-item index="2-1">选项1</el-menu-item>
              <el-menu-item index="2-2">选项2</el-menu-item>
            </el-menu-item-group>
            <el-menu-item-group title="分组2">
              <el-menu-item index="2-3">选项3</el-menu-item>
            </el-menu-item-group>
            <el-submenu index="2-4">
              <template slot="title">选项4</template>
              <el-menu-item index="2-4-1">选项4-1</el-menu-item>
            </el-submenu>
          </el-submenu>

          <el-submenu index="3">
            <template slot="title"
              ><i class="el-icon-setting"></i>导航三</template
            >
            <el-menu-item-group>
              <template slot="title">分组一</template>
              <el-menu-item index="3-1">选项1</el-menu-item>
              <el-menu-item index="3-2">选项2</el-menu-item>
            </el-menu-item-group>
            <el-menu-item-group title="分组2">
              <el-menu-item index="3-3">选项3</el-menu-item>
            </el-menu-item-group>
            <el-submenu index="3-4">
              <template slot="title">选项4</template>
              <el-menu-item index="3-4-1">选项4-1</el-menu-item>
            </el-submenu>
          </el-submenu> -->
        </el-menu>
      </el-aside>

      <el-main>
        <!-- 顶部查询表单 -->
        <el-form :inline="true" :model="formInline" class="demo-form-inline">
          <el-form-item label="题目">
            <el-input
              v-model="formInline.question"
              placeholder="请输入题目关键词"
            ></el-input>
          </el-form-item>

          <el-form-item>
            <el-button type="primary" @click="onSubmit">查询</el-button>
          </el-form-item>

          <el-form-item>
            <el-button type="success" @click="onAddQuestion">添加题目</el-button>
          </el-form-item>
        </el-form>

        <!-- 显示的table -->
        <el-table :data="tableData" style="width: 100%">
          <el-table-column label="序号" width="50" align="center">
            <template slot-scope="scope">
              <!-- <i class="el-icon-time"></i> -->
              <span style="margin-left: 10px">{{ scope.row.id }}</span>
            </template>
          </el-table-column>
          <el-table-column label="题目" width="280" align="center">
            <template slot-scope="scope">
              <!-- <el-popover trigger="hover" placement="top">
                <p>题目: {{ scope.row.question }}</p> -->
                <div slot="reference" class="name-wrapper">
                  <!-- <el-tag size="medium">{{ scope.row.question }}</el-tag> -->
                  <span style="margin-left: 10px">{{ scope.row.question }}</span>
                </div>
              <!-- </el-popover> -->
            </template>
          </el-table-column>

          <el-table-column label="选项" width="350" align="center">
            <template slot-scope="scope">
              <!-- <i class="el-icon-time"></i> -->
              <span style="margin-left: 10px">{{ scope.row.options.join(", ") }}</span>
            </template>
          </el-table-column>

          <el-table-column label="答案" width="100" align="center">
            <template slot-scope="scope">
              <!-- <i class="el-icon-time"></i> -->
              <span style="margin-left: 10px">{{ scope.row.answer }}</span>
            </template>
          </el-table-column>

          <el-table-column label="操作" align="center">
            <template slot-scope="scope">
              <el-button
                size="mini"
                @click="handleEdit(scope.$index, scope.row)">编辑</el-button>
              <el-button
                size="mini"
                type="danger"
                @click="handleDelete(scope.$index, scope.row)">删除</el-button>
            </template>
          </el-table-column>
        </el-table>

        <br />
        <!-- 分页 -->
        <el-pagination background layout="prev, pager, next" :total="1000">
        </el-pagination>

        <!-- 弹出的添加用户表单 -->
        <el-dialog title="添加新题目" :visible.sync="dialogFormVisible">
          <el-form :model="form">
            <el-form-item label="题目" :label-width="formLabelWidth">
              <el-input v-model="form.question" autocomplete="off"></el-input>
            </el-form-item>
            <el-form-item label="选项a" :label-width="formLabelWidth">
              <el-input v-model="form.option1" autocomplete="off"></el-input>
            </el-form-item>
            <el-form-item label="选项b" :label-width="formLabelWidth">
              <el-input v-model="form.option2" autocomplete="off"></el-input>
            </el-form-item>
            <el-form-item label="选项c" :label-width="formLabelWidth">
              <el-input v-model="form.option3" autocomplete="off"></el-input>
            </el-form-item>
            <el-form-item label="选项d" :label-width="formLabelWidth">
              <el-input v-model="form.option4" autocomplete="off"></el-input>
            </el-form-item>
            <el-form-item label="答案" :label-width="formLabelWidth">
              <el-input v-model="form.answer" autocomplete="off"></el-input>
            </el-form-item>
          </el-form>
          <div slot="footer" class="dialog-footer">
            <el-button @click="dialogFormVisible = false">取 消</el-button>
            <el-button type="primary" @click="dialogFormVisible = false">确 定</el-button>
          </div>
        </el-dialog>
      </el-main>
    </el-container>
  </el-container>
</template>

<script>
export default {
  data() {
    return {
      dialogFormVisible: false,
      formInline: {
        question: "",
        option1: "",
        option2: "",
        option3: "",
        option4: "",
        answer: "",
      },
      tableData: [
        {
          id: "1",
          question: "巴黎的首都是哪一座城市？",
          options: [
            "巴黎",
            "巴黎",
            "巴黎",
            "巴黎",
          ],
          answer: "巴黎",
        },
        {
          id: "2",
          question: "巴黎的首都是哪一座城市？",
          options: [
            "巴黎",
            "巴黎",
            "巴黎",
            "巴黎",
          ],
          answer: "巴黎",
        },
        {
          id: "3",
          question: "巴黎的首都是哪一座城市？",
          options: [
            "巴黎",
            "巴黎",
            "巴黎",
            "巴黎",
          ],
          answer: "巴黎",
        },
        {
          id: "4",
          question: "巴黎的首都是哪一座城市？",
          options: [
            "巴黎",
            "巴黎",
            "巴黎",
            "巴黎",
          ],
          answer: "巴黎",
        },
      ],
      form: {
        question: "",
        option1: "",
        option2: "",
        option3: "",
        option4: "",
        answer: "",
      },
    };
  },
  methods: {
    onAddQuestion() {
      this.dialogFormVisible = true;
    },
    onSubmit() {
      console.log("submit!");
    },
    handleEdit(index, row) {
      console.log(index, row);
    },
    handleDelete(index, row) {
      console.log(index, row);
    },
  },
};
</script>

<style>
/* .el-header, .el-footer {
    background-color: #B3C0D1;
    color: #333;
    text-align: center;
    line-height: 60px;
  }
  
  .el-aside {
    background-color: #D3DCE6;
    color: #333;
    text-align: center;
    line-height: 200px;
  }
  
  .el-main {
    background-color: #E9EEF3;
    color: #333;
    text-align: center;
    line-height: 160px;
  } */
</style>