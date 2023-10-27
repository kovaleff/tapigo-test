<script setup>
import {ref, reactive} from 'vue'
import postsdata from '../seed'

const dialogVisible = ref(false)
const commentFormVisible = ref(false)
const currentRow = ref(0)
const comment = ref('')

const posts = reactive(postsdata)


const handleListClick = (index) =>{
  dialogVisible.value = true
  currentRow.value = index;
}

const addCommentHandle = (index) =>{
  commentFormVisible.value = true
  currentRow.value = index;
}

const addCommentToRow = () =>{
  commentFormVisible.value = true
  // posts.data[currentRow.value].comments.push(comment)
  posts.data[currentRow.value].comments = [...posts.data[currentRow.value].comments, comment.value]
  comment.value = '';
  commentFormVisible.value = false
}

const addCommentToComment = () =>{
  commentFormVisible.value = true
}


</script>

<template>
  <h1>List:</h1>
  <el-row class="mb-10">
    <el-table :data="posts.data" style="width: 100%">
      <el-table-column prop="avatar" label="Avatar">
        <template #default="scope">
          <img :src="scope.row.avatar" alt="" class="max-width100 avatar"  @click="handleListClick(scope.$index)">
        </template>
      </el-table-column>
      <el-table-column prop="title" label="Title" width="180" />
      <el-table-column prop="description" label="Description" width="180" />

      <el-table-column label="Comments">
        <template #default="scope">
          <ul>
            <el-tag class="ml-2" type="success" v-for="comment in posts.data[scope.$index].comments" @click="addCommentToComment">{{comment}}</el-tag>
          </ul>

        </template>
      </el-table-column>

      <el-table-column label="Operations">
        <template #default="scope">
          <el-button size="small" @click="addCommentHandle(scope.$index)">Comment</el-button>
        </template>
      </el-table-column>

    </el-table>
  </el-row>

  <el-dialog
      v-model="dialogVisible"
      title="Post detail"
      width="30%">

    <el-card :body-style="{ padding: '0px' }">
      <img :src="posts.data[currentRow].avatar" class="image"/>
      <div style="padding: 14px">
        <span>{{posts.data[currentRow].title}}</span>
        <div class="bottom">
          <el-button text class="button">{{posts.data[currentRow].description}}</el-button>
        </div>
      </div>
    </el-card>

    <template #footer>
      <span class="dialog-footer">
        <el-button @click="dialogVisible = false">Cancel</el-button>
      </span>
    </template>
  </el-dialog>

  <el-dialog
      v-model="commentFormVisible"
      title="Add a comment"
      width="30%">
    <el-input
        v-model="comment"
        :rows="2"
        type="textarea"
        placeholder="add a comment"
    />

    <template #footer>
      <span class="dialog-footer">
        <el-button @click="commentFormVisible = false">Cancel</el-button>
        <el-button type="primary" @click="addCommentToRow">Add</el-button>
      </span>
    </template>
  </el-dialog>

</template>

