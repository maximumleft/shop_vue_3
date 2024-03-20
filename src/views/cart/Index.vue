<script>
export default {
  methods: {
    subTotal(product){
      return product.price * product.qty
    },
    qtyPlus(product){
      product.qty += 1
    },
    qtyMinus(product){
      product.qty -= 1
    },
    getCartProducts() {
      this.products = JSON.parse(localStorage.getItem('cart'))
      this.products.forEach(product => {
        this.getProduct(product.id, product.qty)
      })
      // this.second_products.forEach(product =>{
      //   if(!product.title)
      //
      // })
      //this.second_products = this.second_products.filter((product) => product.qty !== '');
      console.log(this.second_products);
      console.log(this.products);

    },
    getProduct(id, qty) {
      this.axios.get(`http://127.0.0.1:8000/api/products/${id}`)
          .then(res => {
            this.second_products.push(res.data.data)
            this.second_products[id].qty = qty
          })
    },
    storeOrder() {
      this.axios.post('http://127.0.0.1:8000/api/orders', {
        'products': this.products,
        'name': this.name,
        'email': this.email,
        'address': this.address,
        'total_price': this.totalPrice,

      })
          .then(res => {
            console.log(res);
          })
    },

  },
  computed: {
    totalPrice() {
      let total
      this.products.forEach(product =>
          total += product.price
      )
      return total
    }
  },
  data() {
    return {
      products: [],
      second_products: [{
        qty: ''
      }],
      name: '',
      email: '',
      address: '',
    }
  },
  mounted() {
    this.getCartProducts();
  }
}

</script>

<template>
  <div>
    <main class="overflow-hidden ">
      <!--Start Breadcrumb Style2-->
      <section class="breadcrumb-area" style="background-image: url(@/assets/images/inner-pages/breadcum-bg.png);">
        <div class="container">
          <div class="row">
            <div class="col-xl-12">
              <div class="breadcrumb-content text-center wow fadeInUp animated">
                <h2>Cart</h2>
                <div class="breadcrumb-menu">
                  <ul>
                    <li><a href="index.html"><i class="flaticon-home pe-2"></i>Home</a></li>
                    <li><i class="flaticon-next"></i></li>
                    <li class="active">Cart</li>
                  </ul>
                </div>
              </div>
            </div>
          </div>
        </div>
      </section>
      <!--End Breadcrumb Style2-->
      <!--Start cart area-->
      <section class="cart-area pt-120 pb-120">
        <div class="container">
          <div class="row wow fadeInUp animated">
            <div class="col-xl-12 col-lg-12 col-md-12 col-sm-12">
              <div class="cart-table-box">
                <div class="table-outer">
                  <table class="cart-table">
                    <thead class="cart-header">
                    <tr>
                      <th class="">Product Name</th>
                      <th class="price">Price</th>
                      <th>Quantity</th>
                      <th>Subtotal</th>
                      <th class="hide-me"></th>
                    </tr>
                    </thead>
                    <tbody>
                    <tr v-for="product in this.second_products">
                      <td>
                        <div class="thumb-box"><a href="shop-details-1.html" class="thumb">
                          <img :src="product.preview_image" alt="">
                        </a> <a href="shop-details-1.html" class="title">
                          <h5> {{ product.title }} </h5>
                        </a></div>
                      </td>
                      <td>${{ product.price }}</td>
                      <td class="qty">
                        <div class="qtySelector text-center"> <span class="decreaseQty"><i
                            @click.prevent="qtyMinus(product)"
                            class="flaticon-minus"></i> </span>
                          <input type="number" class="qtyValue" :value="product.qty"/> <span
                            class="increaseQty"> <i @click.prevent="qtyPlus(product)"
                            class="flaticon-plus"></i> </span></div>
                      </td>
                      <td class="sub-total">${{this.subTotal(product)}}</td>
                      <td>
                        <div class="remove"><i class="flaticon-cross"></i></div>
                      </td>
                    </tr>
                    </tbody>
                  </table>
                </div>
              </div>
            </div>
          </div>
          <div class="row w-25">
            <input type="text" v-model="name" placeholder="name">
            <input type="text" v-model="email" placeholder="email">
            <input type="text" v-model="address" placeholder="address">
            <input @click.prevent="storeOrder" type="submit" class="btn btn-primary" value="Оформить">
          </div>
          <div class="row pt-120">
            <div class="col-xl-6 col-lg-7 wow fadeInUp animated">
              <div class="cart-total-box">
                <div class="inner-title">
                  <h3>Cart Totals</h3>
                </div>
              </div>
            </div>
          </div>
          <div class="row mt--30">
            <div class="col-xl-6 col-lg-7 wow fadeInUp animated">
              <div class="cart-total-box mt-30">
                <div class="table-outer">
                  <table class="cart-table2">
                    <thead class="cart-header clearfix">
                    <tr>
                      <th colspan="1" class="shipping-title">Shipping</th>
                      <th class="price">$2500.00</th>
                    </tr>
                    </thead>
                    <tbody>
                    <tr>
                      <td class="shipping"> Shipping</td>
                      <td class="selact-box1">
                        <ul class="shop-select-option-box-1">
                          <li><input type="checkbox" name="free_shipping" id="option_1"
                                     checked=""> <label for="option_1"><span></span>Free
                            Shipping</label></li>
                          <li><input type="checkbox" name="flat_rate" id="option_2"> <label
                              for="option_2"><span></span>Flat Rate</label></li>
                          <li><input type="checkbox" name="local_pickup" id="option_3">
                            <label for="option_3"><span></span>Local Pickup</label></li>
                        </ul>
                        <div class="inner-text">
                          <p>Shipping options will be updated during checkout</p>
                        </div>
                        <h4>Calculate Shipping</h4>
                      </td>
                    </tr>
                    <tr>
                      <td>
                        <h4 class="total">Total</h4>
                      </td>
                      <td class="subtotal">$2500.00</td>
                    </tr>
                    </tbody>
                  </table>
                </div>
              </div>
            </div>
            <div class="col-xl-6 col-lg-5 wow fadeInUp animated">
              <div class="cart-check-out mt-30">
                <h3>Check Out</h3>
                <ul class="cart-check-out-list">
                  <li>
                    <div class="left">
                      <p>Subtotal</p>
                    </div>
                    <div class="right">
                      <p>$2500.00</p>
                    </div>
                  </li>
                  <li>
                    <div class="left">
                      <p>Shipping</p>
                    </div>
                    <div class="right">
                      <p><span>Flat rate:</span> $50.00</p>
                    </div>
                  </li>
                  <li>
                    <div class="left">
                      <p>Total Price:</p>
                    </div>
                    <div class="right">
                      <p>$2550.00</p>
                    </div>
                  </li>
                </ul>
              </div>
            </div>
          </div>
        </div>
      </section>
      <!--End cart area-->
    </main>
  </div>
</template>

<style scoped>

</style>