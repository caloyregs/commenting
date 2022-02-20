
<template>
    <li class="list-group-item">
        <b>{{ comment.name }}</b> said {{ comment.comment }}
        
        <a @click="replyComment = comment" v-if="comment.depth!=3" class="reply-btn">Reply</a>
        
        <comment-form v-if="replyComment == comment" :comment="comment"></comment-form>

        <comment-list v-if="collection[comment.id]" v-bind:comments="collection[comment.id]" v-bind:collection="collection">
        </comment-list>
        
    </li>
</template>

<script>
    import CommentList from './CommentList.vue';
    import CommentForm from './CommentForm.vue';

    export default{
        props: ['comment', 'collection'],
        data(){
            return {
                replyComment: false
            }
        },
        mounted(){
        },
        components: {
            'comment-list': CommentList,
            'comment-form': CommentForm
        }
    }
</script>
