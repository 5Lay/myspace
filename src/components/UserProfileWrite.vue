<template>
    <div class="card edit-field">
        <div class="card-body">
            <!-- <label for="edit-post" class="form-label">编辑帖子</label> -->
            <textarea v-model="content" class="form-control" placeholder="发表此刻心情~" id="edit-post" style="height: 100px"></textarea>
            <button @click="publish" type="button" class="btn btn-primary btn-sm">发帖</button>
        </div>
    </div>
</template>


<script>
import { ref } from 'vue';
import $  from 'jquery';
import { useStore } from 'vuex';

export default {
    name: "UserProfileWrite",
    setup(props, context) {
        const store = useStore();
        let content = ref('');

        const publish = () => {
            $.ajax({
                url: "https://app165.acapp.acwing.com.cn/myspace/post/",
                type: "POST",
                data: {
                    content: content.value,
                },
                headers: {
                    'Authorization': "Bearer " + store.state.user.access,  
                },
                success(resp) {
                    if (resp.result === "success") {
                        context.emit('publish', content.value);
                        content.value = "";     
                    }    
                }
            });
        };

        return {
            content,
            publish,
        }
    }
}
</script>

<style scoped>
.edit-field {
    margin-top: 20px;
}

button {
    margin-top: 10px;
}
</style>