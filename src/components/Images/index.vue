<template>
  <div>
    <el-button type="primary" class="filter-item" icon="el-icon-refresh" @click="handleRefresh">刷新</el-button>
    <el-table :data="data" tooltip-effect="dark" style="width: 96%" border fit @cell-dblclick="handleSelect">
      <el-table-column label="文件名" width="250" prop="filename" />
      <el-table-column prop="url" label="预览" width="150">
        <template slot-scope="attachment">
          <el-image
            v-if="attachment.row.file_ext === 'jpg' || attachment.row.file_ext === 'jpeg' || attachment.row.file_ext === 'png' || attachment.row.file_ext === 'gif'"
            style="width: 100px;"
            :src="attachment.row.url"
            :preview-src-list="[attachment.row.url]">
          </el-image>
          <el-tag v-else class="el-icon-download" @click="download(attachment.row.url)">下载</el-tag>
        </template>
      </el-table-column>
      <el-table-column prop="path" label="路径" width="330" />
      <el-table-column prop="file_ext" label="文件后缀" width="100">
        <template slot-scope="attachment">
          <el-tag type="success" @click="queryParam.file_ext=attachment.row.file_ext;handleSearch()">{{ attachment.row.file_ext }}</el-tag>
        </template>
      </el-table-column>
      <el-table-column prop="file_size" label="文件大小" width="100">
        <template slot-scope="attachment">
          <el-tag type="warning">{{ parseInt(attachment.row.file_size/1024) }}KB</el-tag>
        </template>
      </el-table-column>
      <el-table-column prop="mime_type" label="MimeType" width="100">
        <template slot-scope="attachment">
          <el-tag @click="queryParam.mime_type=attachment.row.mime_type;handleSearch()">{{ attachment.row.mime_type }}</el-tag>
        </template>
      </el-table-column>
      <el-table-column prop="driver" label="驱动" width="100">
        <template slot-scope="attachment">
          <el-tag type="danger" @click="queryParam.driver=attachment.row.driver;handleSearch()">{{ attachment.row.driver }}</el-tag>
        </template>
      </el-table-column>
    </el-table>
    <el-pagination
      background
      @size-change="handleSizeChange"
      @current-change="handleCurrentChange"
      :current-page="paginate.current"
      hide-on-single-page
      :page-sizes="paginate.sizes"
      :page-size="paginate.limit"
      :layout="paginate.layout"
      :total="paginate.total"/>
  </div>
</template>

<script>
import formOperate from '@/layout/mixin/formOperate'
export default {
  mixins: [formOperate],
  data() {
    return {
      url: 'attachments',
      title: '选择附件',
      queryParam: {
        file_ext: '',
        mime_type: '',
        driver: ''
      }
    }
  },
  methods: {
    download(url) {
      location.href = url
    },
    handleSelect(row, column, cell, event) {
      this.$emit('selectAttach', row.url)
    }
  }
}
</script>

<style scoped>

</style>
