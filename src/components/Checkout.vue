<script>
export default {
    props:['cart',],
    data() {
        return {
            passCheck:'',
            nameCheck:'',
            nameCheckValue:'',
            name:'',
            phone:'',
            passCheckValue:''
        }
    },
    computed:{
             canCheckout(){
             if(this.nameCheckValue && this.passCheckValue){
               return true;
             }
             return false;
           },
            checkName(){
            var regex = /^[a-zA-Z ]{2,30}$/;

            if(regex.test(this.name)){
              this.nameCheckValue = true;
              return true;
            }else {
              this.nameCheckValue = false;
             return false
            }
           },
           checkRegex(){
            var reg = /^[0-9]{9,12}$/;
            
            if(reg.test(this.phone)){
              this.passCheckValue = true;

              return true;
            }else{
              this.passCheckValue = false;
              return false;
            }
            
           },
    },
    methods: {
         checkout(name,phone){
        this.$emit('checkout',name,phone)
    },
    removeFromCart(lesson){
        this.$emit('removeFromCart',lesson)
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
    },
}
</script>

<template>

  <div class="flex justify-between mb-2">

              
      <input type="text" placeholder="Name" v-model="name" class="w-1/2 bg-gray-100 rounded border bg-opacity-50 border-gray-300 focus:ring-2 focus:ring-indigo-200 focus:bg-transparent focus:border-indigo-500 text-base outline-none text-gray-700 py-1 px-3 leading-8 transition-colors duration-200 ease-in-out">
      <span class="w-3/4" v-if="checkName">{{nameCheck}}</span>
      <span v-else class="w-3/4 text-red-500" >Enter a name</span>
      <div class="w-1/2"></div>
      <input type="number" placeholder="Phone" v-model="phone" class="w-1/2 bg-gray-100 rounded border bg-opacity-50 border-gray-300 focus:ring-2 focus:ring-indigo-200 focus:bg-transparent focus:border-indigo-500 text-base outline-none text-gray-700 py-1 px-3 leading-8 transition-colors duration-200 ease-in-out">
     <span class="w-3/4" v-if="checkRegex">{{passCheck}}</span>
     <span v-else class="w-3/4 text-red-500" >Enter a valid number</span>
      <div class="w-1/2"></div>
      <button v-on:click="checkout(name,phone)" v-if="canCheckout" class=" text-white bg-black border-0 py-2 px-6 focus:outline-none hover:bg-black rounded ">Checkout</button>
      <button v-else disabled  class="  text-white bg-black opacity-25 border-0 py-2 px-6 focus:outline-none hover:bg-black rounded " >Checkout</button>
    
    
  </div>
  
  <div class="flex flex-wrap -m-4">
    <div class="p-4 md:w-1/3" v-for="lesson in cart">
      <div class="h-full border-2 border-gray-200 border-opacity-60 rounded-lg overflow-hidden">
        <img class="lg:h-48 md:h-36 w-full object-cover object-top hover:object-bottom duration-500 transition-all " v-bind:src="lesson.image" alt="blog">
        <div class="p-6">
        
          <h1 class="title-font text-lg font-medium  mb-3" v-text="lesson.title"></h1>
          <h1 class="title-font text-lg font-medium  mb-3" v-text="lesson.location"></h1>
          <h1 class="title-font text-lg font-medium  mb-3" >Price:{{formatPrice(lesson.price)}}</h1>
          <h1 class="title-font text-lg font-medium  mb-3" >Quantity:{{lesson.qty}}</h1>
          <button v-on:click="removeFromCart(lesson)" class="flex mt-6 text-white bg-black border-0 py-2 px-6 focus:outline-none hover:bg-black rounded ">Remove</button>

        </div>
      </div>
    </div>
</div>
</template> 