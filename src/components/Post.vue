<template>
    <div class="col-md-6 col-lg-4 mb-4">
        <b-card 
            class="post" 
            :title="title" 
            :sub-title="userName"
        >
            <b-card-text>
                {{body}}
            </b-card-text>
        </b-card>
    </div>
</template>

<script>
export default {
    name:"Post",
    props:{
        id:{
            type:Number,
            required:true
        },
        userId:{
            type:Number,
            required:true
        },
        userName:{
            type:String,
            required:true
        },        
        title:{
            type:String,
            required:true
        },
        body:{
            type:String,
            required:true
        }
    }
}
</script>

<style>
    .post{
        height: 100%;
    }
</style>