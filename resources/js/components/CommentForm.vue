<template>
    <div class="comment-form">
        <input type="text" v-model="name" class="form-control" placeholder="Name" v-on:keyup.enter="$event.target.nextElementSibling.focus()" ref="nn">
        <input type="text" v-model="content" class="form-control" v-on:keyup.enter="replyTo(comment)" placeholder="post comment" ref="cform">
    </div>
</template>
<script>
    export default{
        props: ['comment'],
        data(){
            return{
                errors: [],
                name: '',
                content: ''
            }
        },
        methods:{
            replyTo(comment){
				if(!this.name){					
                    this.$refs.nn.focus();
                    return false;
				}
				if(!this.content){					
					this.$refs.cform.focus();
                    return false;
				}
				if(this.name && this.content){
                        axios.post('/replycomment', {
                            name: this.name,
                            content: this.content, 
                            post_id: comment.post_id, 
                            parent_id: comment.id, 
                            depth: comment.depth}
                        ).then(response => {
                            this.content = '';
                            this.name = '';
                        if (!response.data.error) {
                            this.content = '';
                        this.name = '';
                            let payLoad = {
                                post_id: comment.post_id,
                                comments: response.data.data
                            };
                            this.$store.commit('updateComments',payLoad);
                        }
                    });
                }
            }
        }
    }
</script>
