<script>
import Lesson from './components/Lesson.vue';
import Checkout from './components/Checkout.vue';
export default {
  data(){
    return{
          sitename:'After School Club',
           lessons:[],
            sort:false,
            directionSort:false,
            options:{
                Su:'Subject',
                Lo:'Location',
                Pr:'Price',
                Av:'Availability'
            },
            directions:{
                ASC:'Ascending',
                DESC:'Descending'
            },
            direction:'Ascending',
            option:'Sort',
            search:'',
            cart:[],
            lessonInCart:[],
            componentLoaded:false,
            showLessons:true,
            filteredList:[],
    }
  },
  components: {
    Lesson,
    Checkout
  },
   created: function (){
         this.createLesson();
        },

        methods:{
                
          async createLesson(){
            const response = await fetch('https://coursewok.herokuapp.com/',{
              headers:{
                'Content-Type': 'application/json',
                
              }
            })
            const lessons = await response.json()

            this.lessons = lessons;
         
            this.componentLoaded = true;
     
            console.log('Content Loaded');
          },
            compare_function(obj1, obj2){
             
             if(this.option === 'Subject'){
       
                 if (obj1.title == obj2.title) return 0;
if (obj1.title < obj2.title){
 return -1 
} else {return 1};
             }else if(this.option === 'Location'){
                               
if (obj1.location == obj2.location) return 0;
if (obj1.location < obj2.location){
 return -1 
} else {return 1};
             }else if(this.option === 'Price'){
                               
if (obj1.price == obj2.price) return 0;
if (obj1.price < obj2.price){
 return -1 
} else {return 1};
             }else if(this.option === 'Availability'){
                               
if (obj1.availableSpace == obj2.availableSpace) return 0;
if (obj1.availableSpace < obj2.availableSpace){
 return -1 
} else {return 1};
             }


},

compare_function_desc(obj1, obj2){
             
             if(this.option === 'Subject'){
       
                 if (obj1.title == obj2.title) return 0;
if (obj1.title < obj2.title){
 return 1 
} else {return -1};
             }else if(this.option === 'Location'){
                               
if (obj1.location == obj2.location) return 0;
if (obj1.location < obj2.location){
 return 1 
} else {return -1};
             }else if(this.option === 'Price'){
                               
if (obj1.price == obj2.price) return 0;
if (obj1.price < obj2.price){
 return 1 
} else {return -1};
             }else if(this.option === 'Availability'){
                               
if (obj1.availableSpace == obj2.availableSpace) return 0;
if (obj1.availableSpace < obj2.availableSpace){
 return 1 
} else {return -1};
             }


},
            selectSort(state){
                this.option = state;
                this.checkSort();
                this.sort = false;
            },
            checkSort(){
               const object = this.options;
               for (const key in object) {
               
                  if(this.option === object[key]){
                    if(this.direction === 'Ascending'){
                      if(this.canShowFilteredList){
                        this.filteredList.sort(this.compare_function)
                      }
                      this.lessons.sort(this.compare_function) ;
                  } else if(this.direction === 'Descending'){
                    if(this.canShowFilteredList){
                      this.filteredList.sort(this.compare_function_desc)
                    }
                    this.lessons.sort(this.compare_function_desc)
                  }   
                  }
              }
           },
            showSort(){
               this.sort = !this.sort; 
            },
            selectDirection(direction){
                this.direction = direction;
                this.checkSort()
                this.directionSort = false;
            },
            showDirection(){
               this.directionSort = !this.directionSort; 
            },
            async addToCart(lesson){

              let qty;
              qty = 1;
              if (!this.lessonExists(lesson.id)){
                this.cart.push({
                 "id":lesson.id,
                 "title":lesson.title,
                 "image":lesson.image,
                 "price":lesson.price,
                 "location":lesson.location,
                 "qty":qty
               })
              }else{
  
                let objIndex;
            objIndex = this.cart.findIndex((obj => obj.id == lesson.id));
            this.cart[objIndex].qty += 1
              this.cart = [...this.cart]
          }

                   lesson.space --;
                 
            },
            lessonExists(id){
              return this.cart.some(function(el) {
    return el.id === id;
  }); 
            },
            async removeFromCart(lesson){

             let objIndex;
             let cartIndex;
              objIndex = this.lessons.findIndex((obj => obj.id === lesson.id));
              cartIndex = this.cart.findIndex((obj)=> obj.id === lesson.id)
              const element = this.cart[cartIndex];
     
              this.lessons[objIndex].space += element.qty
              this.cart.splice(cartIndex,1)
            if(this.cart.length == 0){
                       
                       this.showLessons = true;
                   }
          
                   return;
  
             
                   
               
            },
            
            cartItemCount(lessonId){
              count = 0;
   
            for (let index = 0; index < this.cart.length; index++) {
              if(lessonId === this.cart[index].id){
                const element = this.cart[index];
              count = element.qty
    
              }
            
            }
            return count
            } ,
            showCart(){
              this.showLessons = !this.showLessons;
              
            },
            async searchBack(){

              const response = await fetch(`https://coursewok.herokuapp.com/search/?searchTerm=${this.search.toLowerCase()}`)
   
            const filter = await response.json();
            this.filteredList = filter
       
            },
            async checkout(name,phone){
           
             const response = await fetch('https://coursewok.herokuapp.com/order',{
               method:'POST',
               headers: { 'Content-Type': 'application/json' },
               body:JSON.stringify({
                 name:name,
                 phone:phone,
                 order:this.cart
               })
             })
             
             alert('Your order has been submitted.....redirecting you to the home screen!!')
             this.cart = []
             console.log(name,phone)
             this.showLessons = true;
           },
            
             
        },
        computed:{
    
            canShowSort(){
                return this.sort
            },
          
            canShowDirectionSort(){
                return this.directionSort
            },
            canShowFilteredList(){
              return this.search.length > 0 ? this.searchBack() : false
            },
           canShowCart(){
           return   this.cart.length > 0 ? true : false
           },
         
        }
}
</script>

<template>
  <section class="text-white body-font bg-slate-900">
            <div class="container px-5 py-24 mx-auto">
                <div class="flex justify-between">
                    <div class="mb-20 ">
                        <h1 class="sm:text-3xl text-2xl font-medium title-font mb-2 "  v-text="sitename"></h1>
                        <div class="h-1 w-20 bg-black rounded"></div>
                      </div>
                      <div class="flex">
                          <div>
                            <label id="listbox-label" class="block text-sm font-medium "> Sort by </label>
                            <div class="mt-1 relative">
                              <button v-on:click="showSort" type="button" class="relative w-full text-black bg-white border border-gray-300 rounded-md shadow-sm pl-3 pr-10 py-2 text-left cursor-default focus:outline-none focus:ring-1 focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm" aria-haspopup="listbox" aria-expanded="true" aria-labelledby="listbox-label">
                                         <span class="ml-3 block px-5" >{{option}}</span>
                       
                                <span class="ml-3 absolute inset-y-0 right-0 flex items-center pr-2 pointer-events-none">
                              
                                  <svg class="h-5 w-5 text-gray-400" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20" fill="currentColor" aria-hidden="true">
                                    <path fill-rule="evenodd" d="M10 3a1 1 0 01.707.293l3 3a1 1 0 01-1.414 1.414L10 5.414 7.707 7.707a1 1 0 01-1.414-1.414l3-3A1 1 0 0110 3zm-3.707 9.293a1 1 0 011.414 0L10 14.586l2.293-2.293a1 1 0 011.414 1.414l-3 3a1 1 0 01-1.414 0l-3-3a1 1 0 010-1.414z" clip-rule="evenodd" />
                                  </svg>
                                </span>
                              </button>
                          
          
                              <ul v-show="canShowSort"  class="absolute text-black z-10 mt-1 w-full bg-white shadow-lg max-h-56 rounded-md py-1 text-base ring-1 ring-black ring-opacity-5 overflow-auto focus:outline-none sm:text-sm" tabindex="-1" role="listbox" aria-labelledby="listbox-label" aria-activedescendant="listbox-option-3">
                             
                                <li v-on:click="selectSort(state)" v-for="(state,key)  in options" class=" cursor-default text-black relative py-2 pl-3 pr-9" id="listbox-option-0" role="option">
                                  <span class="font-normal text-black ml-1 block px-5" v-bind:value="state">{{state}}</span>
                              
                                  
                                </li>
         
                          
                              </ul>
                            </div>
                          </div>
                       <div>
                           
                        <div class="mt-6 ml-8 relative">
                            <button v-on:click="showDirection" type="button" class="relative w-full bg-white text-black border border-gray-300 rounded-md shadow-sm pl-3 pr-10 py-2 text-left cursor-default focus:outline-none focus:ring-1 focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm" aria-haspopup="listbox" aria-expanded="true" aria-labelledby="listbox-label">
                                       <span class="ml-3 block px-5" >{{direction}}</span>
                     
                              <span class="ml-3 absolute inset-y-0 right-0 flex items-center pr-2 pointer-events-none">
                            
                                <svg class="h-5 w-5 text-gray-400" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20" fill="currentColor" aria-hidden="true">
                                  <path fill-rule="evenodd" d="M10 3a1 1 0 01.707.293l3 3a1 1 0 01-1.414 1.414L10 5.414 7.707 7.707a1 1 0 01-1.414-1.414l3-3A1 1 0 0110 3zm-3.707 9.293a1 1 0 011.414 0L10 14.586l2.293-2.293a1 1 0 011.414 1.414l-3 3a1 1 0 01-1.414 0l-3-3a1 1 0 010-1.414z" clip-rule="evenodd" />
                                </svg>
                              </span>
                            </button>
                        
        
                            <ul v-show="canShowDirectionSort"  class="text-black absolute z-10 mt-1 w-full bg-white shadow-lg max-h-56 rounded-md py-1 text-base ring-1 ring-black ring-opacity-5 overflow-auto focus:outline-none sm:text-sm" tabindex="-1" role="listbox" aria-labelledby="listbox-label" aria-activedescendant="listbox-option-3">
                           
                              <li v-on:click="selectDirection(direction)" v-for="(direction,key)  in directions" class=" cursor-default  relative py-2 pl-3 pr-9" id="listbox-option-0" role="option">
                                <span class="font-normal ml-1 block px-5" v-bind:value="direction">{{direction}}</span>
                            
                                
                              </li>
       
                        
                            </ul>
                          </div>
                       </div>
                        
                      </div>
                      <div class="mt-6">
              
                        <input type="text" placeholder="Search" v-model="search" class="w-full bg-slate-900 rounded border bg-opacity-50 border-gray-300 focus:ring-2 focus:ring-indigo-200 focus:bg-transparent focus:border-indigo-500 text-base outline-none text-gray-700 py-1 px-3 leading-8 transition-colors duration-200 ease-in-out">
                     
                      </div>
                      <div class="">
                        <button v-on:click="showCart" v-if="canShowCart" class="flex mt-6 text-white bg-black border-0 py-2 px-6 focus:outline-none hover:bg-black rounded "> <span class="fa fa-shopping-cart mt-1 mr-2"></span>Cart</button>
                        <button v-else disabled  class="flex mt-6 text-white bg-black opacity-25 border-0 py-2 px-6 focus:outline-none hover:bg-black rounded " ><span class="fa fa-shopping-cart mt-1 mr-2"></span>Cart</button>
                    
                            
                      </div>
                </div>
                <div>

                </div>
             
             <div  v-if="showLessons">
            <Lesson @addToCart="addToCart" :lessons="lessons" :filteredList="filteredList" :canShowFilteredList="canShowFilteredList" />
             </div>
 <!--Checckout--> 
<div v-else >
 
<Checkout :cart="cart" @checkout="checkout" @removeFromCart="removeFromCart"  />
</div>
    
   
            </div>
          </section>
</template>