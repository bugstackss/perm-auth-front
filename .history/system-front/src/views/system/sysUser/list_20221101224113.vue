<template>
  <div>
    <div class="search-div">
      <el-form label-width="70px"
               size="small">
        <el-row>
          <el-col :span="8">
            <el-form-item label="关 键 字">
              <el-input style="width: 95%"
                        v-model="searchObj.keyword"
                        placeholder="用户名/姓名/手机号码"></el-input>
            </el-form-item>
          </el-col>
          <el-col :span="8">
            <el-form-item label="操作时间">
              <el-date-picker v-model="createTimes"
                              type="datetimerange"
                              range-separator="至"
                              start-placeholder="开始时间"
                              end-placeholder="结束时间"
                              value-format="yyyy-MM-dd HH:mm:ss"
                              style="margin-right: 10px;width: 100%;" />
            </el-form-item>
          </el-col>
        </el-row>
        <el-row style="display:flex">
          <el-button type="primary"
                     icon="el-icon-search"
                     size="mini"
                     @click="fetchData()">搜索</el-button>
          <el-button icon="el-icon-refresh"
                     size="mini"
                     @click="resetData">重置</el-button>
        </el-row>
      </el-form>
    </div>

    <!-- 工具条 -->
    <div class="tools-div">
      <el-button type="success"
                 icon="el-icon-plus"
                 size="mini"
                 @click="add">添 加</el-button>
    </div>

    <!-- 列表 -->
    <el-table v-loading="listLoading"
              :data="list"
              stripe
              border
              style="width: 100%;margin-top: 10px;">

      <el-table-column label="序号"
                       width="70"
                       align="center">
        <template slot-scope="scope">
          {{ (page - 1) * limit + scope.$index + 1 }}
        </template>
      </el-table-column>

      <el-table-column prop="username"
                       label="用户名"
                       width="180" />
      <el-table-column prop="name"
                       label="姓名"
                       width="110" />
      <el-table-column prop="phone"
                       label="手机" />
      <el-table-column label="状态"
                       width="80">
        <template slot-scope="scope">
          <el-switch v-model="scope.row.status === 1"
                     @change="switchStatus(scope.row)">
          </el-switch>
        </template>
      </el-table-column>
      <el-table-column prop="createTime"
                       label="创建时间" />

      <el-table-column label="操作"
                       align="center"
                       fixed="right">
        <template slot-scope="scope">
          <el-button type="primary"
                     icon="el-icon-edit"
                     size="mini"
                     @click="edit(scope.row.id)"
                     title="修改" />
          <el-button type="danger"
                     icon="el-icon-delete"
                     size="mini"
                     @click="removeDataById(scope.row.id)"
                     title="删除" />
        </template>
      </el-table-column>
    </el-table>

    <!-- 分页组件 -->
    <el-pagination :current-page="page"
                   :total="total"
                   :page-size="limit"
                   style="padding: 30px 0; text-align: center;"
                   layout="total, prev, pager, next, jumper"
                   @current-change="fetchData" />

    <el-dialog title="添加/修改"
               :visible.sync="dialogVisible"
               width="40%">
      <el-form ref="dataForm"
               :model="sysUser"
               label-width="100px"
               size="small"
               style="padding-right: 40px;">
        <el-form-item label="用户名"
                      prop="username">
          <el-input v-model="sysUser.username" />
        </el-form-item>
        <el-form-item v-if="!sysUser.id"
                      label="密码"
                      prop="password">
          <el-input v-model="sysUser.password"
                    type="password" />
        </el-form-item>
        <el-form-item label="姓名"
                      prop="name">
          <el-input v-model="sysUser.name" />
        </el-form-item>
        <el-form-item label="手机"
                      prop="phone">
          <el-input v-model="sysUser.phone" />
        </el-form-item>
      </el-form>
      <span slot="footer"
            class="dialog-footer">
        <el-button @click="dialogVisible = false"
                   size="small"
                   icon="el-icon-refresh-right">取 消</el-button>
        <el-button type="primary"
                   icon="el-icon-check"
                   @click="saveOrUpdate()"
                   size="small">确 定</el-button>
      </span>
    </el-dialog>
  </div>
</template>

<script>
export default {

}
</script>
