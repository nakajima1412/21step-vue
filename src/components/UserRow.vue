<template>
  <tr>
    <td>
      <span v-if="!editable" @click="edit()">{{ user.nickname }}</span>
      <input
        v-show="editable"
        ref="editNickname"
        :user="user"
        @input="this.$emit('updateUserNickName', $event.target.value)"
        @blur="editable = false"
      />
    </td>
    <td>{{ user.email }}</td>
  </tr>
</template>

<script lang="ts">
import { defineComponent, nextTick, ref, PropType } from 'vue'

export interface User {
  nickname: string
  email: string
}

export default defineComponent({
  props: {
    user: {
      type: Object as PropType<User>,
      required: true,
    },
  },
  setup() {
    const editable = ref(false)
    const editNickname = ref<HTMLFormElement | null>(null)

    const edit = () => {
      editable.value = true
      nextTick(() => {
        if (!editNickname.value) {
          return
        }
        // DOM更新後に実行
        editNickname.value.focus()
      })
    }
    return {
      editable,
      editNickname,
      edit,
    }
  },
})
</script>

<style module>
td input {
  width: 95%;
}
</style>
