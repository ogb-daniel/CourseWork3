<script>
export default {
    props: ['lessons','filteredList','canShowFilteredList'],
  methods:{
    addToCart(lesson){
        this.$emit('addToCart',lesson)
    },
    formatPrice(price){
                if(!parseInt(price)){return "";}
                if(price > 99999){
                    var priceString = (price/100).toFixed(2);
                    var priceArray = priceString.split("").reverse();
                    var index = 3;
                    while(priceArray.length > (index + 3)){
                        priceArray.splice((index+3),0,",");
                        index += 4;
                    }
                    return "$"+ priceArray.reverse().join("");
                }else{
                    return "$" + (price/100).toFixed(2);
                }
            },
             canAddToCart:function(lesson){
                return lesson.space > 0 
            },

        },
        computed:{
            
        }
}
</script>

<template>
               <div class="flex flex-wrap -m-4" v-if="canShowFilteredList" >
                 
                <div class="p-4 md:w-1/3"  v-for="lesson in filteredList">
                  <div class="h-full border-2 border-gray-200 border-opacity-60 rounded-lg overflow-hidden">
                    <img class="lg:h-48 md:h-36 w-full object-cover object-top hover:object-bottom duration-500 transition-all " v-bind:src="lesson.image" alt="blog">
                    <div class="p-6">
                    
                      <h1 class="title-font text-lg font-medium  mb-3" v-text="lesson.title"></h1>
                      <h1 class="title-font text-lg font-medium  mb-3" v-text="lesson.location"></h1>
                      <h1 class="title-font text-lg font-medium  mb-3" >Price:{{formatPrice(lesson.price)}}</h1>
                      <h1 class="title-font text-lg font-medium  mb-3" >Number of spaces:{{lesson.space}}</h1>
                      
                      <button v-on:click="addToCart(lesson)" v-if="canAddToCart(lesson)" class="flex mt-6 text-white bg-black border-0 py-2 px-6 focus:outline-none hover:bg-black rounded">Add to cart</button>
                <button v-else disabled  class="flex mt-6 text-white bg-black opacity-25 border-0 py-2 px-6 focus:outline-none hover:bg-black rounded">Add to cart</button>
            
                     
                    </div>
                  </div>
                </div>
    </div>
    <div v-else>
      
      <div class="flex flex-wrap -m-4" >
        <div class="p-4 md:w-1/3" v-for="lesson in lessons">
          <div class="h-full border-2 border-gray-200 border-opacity-60 rounded-lg overflow-hidden">
            <img class="lg:h-48 md:h-36 w-full object-cover object-top hover:object-bottom duration-500 transition-all " v-bind:src="lesson.image" alt="blog">
            <div class="p-6">
            
              <h1 class="title-font text-lg font-medium  mb-3" v-text="lesson.title"></h1>
              <h1 class="title-font text-lg font-medium  mb-3" v-text="lesson.location"></h1>
              <h1 class="title-font text-lg font-medium  mb-3" >Price:{{formatPrice(lesson.price)}}</h1>
              <h1 class="title-font text-lg font-medium  mb-3" >Number of spaces:{{lesson.space}}</h1>
              
              <button v-on:click="addToCart(lesson)" v-if="canAddToCart(lesson)" class="flex mt-6 text-white bg-black border-0 py-2 px-6 focus:outline-none hover:bg-black rounded "> <span class="fa fa-plus mt-1 mr-2"></span> Add to cart</button>
        <button v-else disabled  class="flex mt-6 text-white bg-black opacity-25 border-0 py-2 px-6 focus:outline-none hover:bg-black rounded fa fa-plus" ><span class="fa fa-plus mt-1 mr-2"></span> Add to cart</button>
    
             
            </div>
          </div>
        </div>
</div>
</div>
</template>