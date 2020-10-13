<template>
    <div class="product-display">
        <div class="product-container">
          <div class="product-image">
           <img :class="{ 'out-of-stock-img': !inStock }" v-bind:src="require('@/assets/' + image)" />
           
          </div>
          <div class="product-info">
            <h1>{{ title }}</h1>
            <p>{{ sale }}</p>
            <p>{{shipping}}</p>
            <p v-if="inStock">In Stock</p>
            <p v-else>Out of Stock</p>
            
           <!-- <p v-show="inStock">In Stock</p>
            <p v-if="inventory > 10">In Stock</p>
            <p v-else-if="inventory<=10 && inventory>0">Almost Sold Out</p>
            <p v-else>Out of Stock</p>-->

            <ProductDeatails :details="details"></ProductDeatails>
            <div v-for="(variant, index) in variants" :key="variant.id" 
            @mouseover="updateVariant(index)" 
            class="color-circle"
            :style="{backgroundColor: variant.color}"
            >
            </div>
            <button class="button"  
            :class="{disabledButton: !inStock}"
            :disabled="!inStock"
            v-on:click="addToCart"
            >Add to Cart</button>
            <button class="button" v-on:click="removeFromCart">Remove </button>
          <!--coding challenges-->
            <a :href="url">URL</a>
            <p v-if="onSale">On Sale</p>
             <ul>
              <li v-for="size in sizes" :key="size">{{ size }}</li>
            </ul>
            </div>
        </div>
        <ReviewList v-if="reviews.length" :reviews="reviews"></ReviewList>
        <ReviewForm @review-submitted="addReview"></ReviewForm>
      </div>
</template>

<script>
import ProductDeatails from "./ProductDetails";
import ReviewForm from "./ReviewForm";
import ReviewList from "./ReviewList"
export default {
    name: "ProductDisplay",
    components:{
        ProductDeatails,
        ReviewForm,
        ReviewList,
    },
    props: {
        premium: {
            type: Boolean,
            required: true
        }
    },
 data() {
    return {
      product: "Socks",
      brand: "Vue Mastery",
      description: "description",
      selectedVariant: 0,
      url: 'https://www.vuemastery.com/',
     
      inventory: 0,
      onSale: true,
      details: ['50% cotton', '30% wool', '20% polyester'],
      variants:[
        {id:2234, color: "green", image: 'images/socks_green.jpg', quantity: 50},
          {id:2235, color:"blue", image: 'images/socks_blue.jpg', quantity: 0},
      ],
      sizes: ['S', 'M', 'L', 'XL'],
       cart: 0,
       reviews: [],
    };
  },
  methods:{
    addToCart(){
      this.$emit('add-to-cart',this.variants[this.selectedVariant].id);
    },

    updateVariant(index){
      this.selectedVariant=index;
    },
    removeFromCart(){
     this.$emit('remove-from-cart',this.variants[this.selectedVariant].id);
    },
    addReview(review){
        this.reviews.push(review);
    }
  },
  computed:{
  title(){
    return this.brand + ' '+ this.product;
  },
  image(){
    return this.variants[this.selectedVariant].image;
  },
  inStock(){
    return this.variants[this.selectedVariant].quantity;
  },
  sale() {
            if (this.onSale) {
                return this.brand + ' ' + this.product + ' is on sale.'
            }
            return ''
        },
    shipping(){
        if(this.premium){
            return "Free"
        }
        return "2.99"
    }    
  }
}
</script>

<style>

</style>