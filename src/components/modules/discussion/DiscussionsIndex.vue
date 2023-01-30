<template>
  <div>

    <div  v-for="(discussion ,key)  in discussionItems"  :key="discussion.user.name" class="card mb-2">
      <div class="card-body p-2 p-sm-3">
          <div class="media forum-item">
              <a href="#" data-toggle="collapse" data-target=".forum-content">
                <img v-bind:src=" discussion.user.avatar" class="mr-3 rounded-circle" width="50" alt="User"></a>
              <div class="media-body">
                  <h6><a href="#" data-toggle="collapse" data-target=".forum-content" class="text-body">{{ discussion.user.name }}</a></h6>
                  <p class="text-secondary">
                    {{ discussion.text }}
                    
                  </p>
                  <p class="text-muted"><a href="javascript:void(0)">{{ discussion.replies.length }}</a> replied count  | <span class="text-secondary font-weight-bold">{{ key + 5 }} minutes ago</span></p>
              </div>
              <div class="text-muted small text-center align-self-center">
                  <span class="d-none d-sm-inline-block"><i class="far fa-eye"></i> 19</span>
                  <span><i class="far fa-comment ml-2"></i> 3</span>
              </div>
          </div>


          <div  v-if="discussion.replies.length > 0" class="py-4">
            <div class="card py-1 my-2"  v-for="sub_discussion  in discussion.replies"  :key="sub_discussion.user.name"  >
              <div class="media forum-item mx-5">
                <a href="#" data-toggle="collapse" data-target=".forum-content">
                  <img v-bind:src=" sub_discussion.user.avatar" class="mr-3 rounded-circle" width="50" alt="User"></a>
                <div class="media-body">
                    <h6><a href="#" data-toggle="collapse" data-target=".forum-content" class="text-body">{{ sub_discussion.user.name }}</a></h6>
                    <p class="text-secondary">
                      {{ sub_discussion.text }}
                      
                    </p>
                </div>
            </div>
            </div>
        </div>
          
      </div>
  </div>

  </div>
</template>

<script>
  
  
  import axios from 'axios';
  import discussionTemplate from "./Json/discussionTemplate.json";


  export default {
      name: 'DiscussionsIndex',
      props: [ 'discussionable_id' , 'discussionable_type'  , 'DiscussionTemplateJson'],

      data() {
        return {
          discussionItems : [],
          status : "",
        };
      },


      mounted()
      {
        // get discussion items with phoymorphic relationship 

        this.getDiscussionItems()
      }, 

      methods:{
        
        getDiscussionItems()
        {
          if (  this.discussionable_id != null , this.discussionable_type != null){

            // get data form database with axios | fetch method 

            axios.get('/v1/discussions', {
                params: {
                  able_id   : this.discussionable_id , 
                  able_type : this.discussionable_type
                }
              })
              .then(function (response) {
                console.log(response);
                this.discussionItems = response.data ; 
              })


              // if not found any discussion show msg for user ; 
              if(this.discussionItems == [])
                this.status = "your can set first discussion now .. :)" 
          }

          else {

            // if this.DiscussionTemplateJson == true => Using template data and read file json 
            if(this.DiscussionTemplateJson == "true" )
              this.discussionItems = discussionTemplate ; 
            
          }
        }, 

      }

  }

</script>

