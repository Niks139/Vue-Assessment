<template>

   
   <div style="height: 60px;background: white; color: white">
       
   </div>
    
    <div class="body">
    <br>
    <div class="container">
        
        <div class="row flex">
            <div class="col">
                <div class="d-flex justify-content-around overflow-auto">
                    <span>
                        <i class="fa-solid fa-magnifying-glass"></i>
                        <input type="search" v-model="search" placeholder="Find customer">
                    </span>
                    
                    <select class="d-flex" v-model="choice" style="float: end; width: 200px" aria-placeholder="Sort by name"> 
                        <option v-for="option in options" v-bind:value="option.id" v-bind:key="option.value">{{option.value}}</option>
                    </select>
                </div>
            </div>
        </div>
        
            <div class="row flex" v-if="choice!==1 && choice!==2">
                <div  v-for="user in filterSearch" :key="user.id" class="card mx-3 my-3" style="width: 18rem">
                    <div  class="card-body">
                            <h4 class="card-title mb-0"><span class="name">{{user.name}}</span></h4> 
                            <span class="mt-0 uname" >@{{user.username}}</span> <br>
                            <span class="my-4 phrase">"{{user.company.catchPhrase}}"</span> <br>
                            <span > <i class="icon fa-solid fa-envelope"></i> {{user.email}}</span> <br>
                            <span> <i class="icon fa-solid fa-location-dot"></i> {{user.address.street}},{{user.address.suite}}, <br>
                                {{user.address.city}},{{user.address.zipcode}}, <br>
                                {{user.address.geo.lat}},{{user.address.geo.lng}}
                            </span> <br>
                            <span><i class="icon fa-solid fa-phone"></i> {{user.phone}}</span> <br>
                            <span><i class="icon fa-solid fa-globe"></i>{{user.website}}</span> <br>
                            <span><i class="icon fa-solid fa-briefcase"></i>{{user.company.name}}</span> <br>
                            <span class="web"><img src="Vector.png" class="icon"> {{user.company.bs}}</span> 
                    </div>   
                </div>
            </div>    
                    <div class="row flex" v-if="choice===1">
                        <div v-for="user in sortedArray" :key="user.id" class="card mx-3 my-3" style="width: 18rem">
                       <div class="card-body">
                            <h4 class="card-title mb-0"><span class="name">{{user.name}}</span></h4> 
                            <span class="mt-0 uname" >@{{user.username}}</span> <br>
                            <span class="my-4 phrase">"{{user.company.catchPhrase}}"</span> <br>
                            <span > <i class="icon fa-solid fa-envelope"></i> {{user.email}}</span> <br>
                            <span> <i class="icon fa-solid fa-location-dot"></i> {{user.address.street}},{{user.address.suite}}, <br>
                                {{user.address.city}},{{user.address.zipcode}}, <br>
                                {{user.address.geo.lat}},{{user.address.geo.lng}}
                            </span> <br>
                            <span><i class="icon fa-solid fa-phone"></i> {{user.phone}}</span> <br>
                            <span><i class="icon fa-solid fa-globe"></i>{{user.website}}</span> <br>
                            <span><i class="icon fa-solid fa-briefcase"></i>{{user.company.name}}</span> <br>
                            <span class="web"><img src="Vector.png" class="icon"> {{user.company.bs}}</span> 
                        </div>
                    </div>  
                    </div>
                       <div class="row flex" v-if="choice===2">
                         <div v-for="user in UnsortedArray" :key="user.id" class="card mx-3 my-3" style="width: 18rem">
                       <div class="card-body">
                            <h4 class="card-title mb-0"><span class="name">{{user.name}}</span></h4> 
                            <span class="mt-0 uname" >@{{user.username}}</span> <br>
                            <span class="my-4 phrase">"{{user.company.catchPhrase}}"</span> <br>
                            <span > <i class="icon fa-solid fa-envelope"></i> {{user.email}}</span> <br>
                            <span> <i class="icon fa-solid fa-location-dot"></i> {{user.address.street}},{{user.address.suite}}, <br>
                                {{user.address.city}},{{user.address.zipcode}}, <br>
                                {{user.address.geo.lat}},{{user.address.geo.lng}}
                            </span> <br>
                            <span><i class="icon fa-solid fa-phone"></i> {{user.phone}}</span> <br>
                            <span><i class="icon fa-solid fa-globe"></i>{{user.website}}</span> <br>
                            <span><i class="icon fa-solid fa-briefcase"></i>{{user.company.name}}</span> <br>
                            <span class="web"><img src="Vector.png" class="icon"> {{user.company.bs}}</span> 
                        </div>
                    </div> 
            </div>
        </div>
    </div>
</template>

<script>

export default {
  name: 'Users',

  data(){
   return {
        search: '',
        users:[],
        sortType: '',
        el: "#app",
        choice:undefined,
         options:[
             {id:0, value:"Filter by Name (A-Z)"},
             {id:1, value:"Name A-Z"},
             {id:2, value:"Name Z-A"} 
             ],

   }
   },
    methods: {
        sortBy(e) {
          this.sortType = e.target.value;
        },
        onChange() {
            this.sortType = "true"
            console.log('clicked', this.sortType)
        },
        changeAsc() {
            this.sortType = "asc"
            console.log("Asc called", this.sortType)
        },
        changeDesc() {
            this.sortType = "desc"
            console.log("Desc called", this.sortType)
        }

    },
    mounted(){
    fetch('http://jsonplaceholder.typicode.com/users')
        .then(res=>res.json())
        .then(data=>this.users=data)
        .catch(err=>console.log(err.message))
    },

    computed: {

        filterSearch() {
        
        if (this.search || this.choice===1) {
            return this.users.filter(u => u.name.toLowerCase().includes(this.search.toLowerCase()));
        }
        else{
            return this.users;
            
        }
    },
        
    sortedArray() {
           const { users } = this;
            function compare(a, b) {
            if (a.name < b.name)
                return -1;
            if (a.name > b.name)
                return 1;
            return 0;
        }
      return users.sort(compare);
    },
       UnsortedArray() {
        const { users } = this;
        function compare(a, b) {
            if (b.name < a.name)
                return -1;
            if (b.name < a.name)
                return 1;
            return 0;
      }
      return users.sort(compare);
    }
    }
}
</script>

<style scoped>
    span ::first-letter {
        text-transform: uppercase;
        }

    .body{
        background-color: #F2F5FA;
        
    }

    .flex {
        display: flex;
        flex-direction: row;
    }
    .icon {
        color: #015989;
        width: 20px;
        height: 20px;
        font-size: medium;
    }
    span {
        font-family: 'Rubik', sans-serif;
        font-style: normal;
        font-weight: normal;
        font-size: 12px;
        line-height: 20px;
    }
    .name{
        font-family: 'Rubik', sans-serif;
        font-style: normal;
        font-weight: 500;
        font-size: 20px;
        line-height: 28px;
        color: #202223;
    }
    .uname{
        color: #919191;
        font-family: 'Rubik', sans-serif;
        font-style: normal;
        font-weight: normal;
        font-size: 10px !important;
        line-height: 20px;
    }

    .phrase {
        font-style: normal;
        font-weight: normal;
        font-size: 12px;
        line-height: 20px;
        color: #51C5FF;
    }
        
    input{
        border: none;
        outline: none;
        height: 29px !important;
        border-top-right-radius: 5px;
        border-bottom-right-radius: 5px;
    }

    .fa-magnifying-glass{
        background-color: white;
        margin-top: 2px;
        padding: 8px;
        height: 29px !important;
        border-top-left-radius: 5px;
        border-bottom-left-radius: 5px;
    }

    select {
        border: none;
        outline: none;
    }


</style>