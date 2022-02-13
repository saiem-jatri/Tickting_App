<script>
export default{
  data(){
    return{
      err_msg:false,
      appliedCoupon: null,
      couponCode: "",
      coupons: [
        {
          code: "100TAKAOFF",
          discount: 100
        },
        {
          code: "200TAKAOFF",
          discount: 200
        }
      ],
      seatStates: {
        sold: {
          text: "Sold",
          color: "#f60303"
        },
        available: {
          text: "Available",
          color: "#fff"
        },
        booked: {
          text: "Booked",
          color: "gray"
        },
        selected: {
          text: "Selected",
          color: "#00ff00"
        }
      },
      seats: [
        {
          name: "A1",
          type: "available",
          price: 500
        },
        {
          name: "A2",
          type: "available",
          price: 500
        },
        {
          name: "A3",
          type: "available",
          price: 500
        },
        {
          name: "A4",
          type: "available",
          price: 500
        },
        {
          name: "B1",
          type: "available",
          price: 450
        },
        {
          name: "B2",
          type: "available",
          price: 450
        },
        {
          name: "B3",
          type: "available",
          price: 450
        },
        {
          name: "B4",
          type: "available",
          price: 450
        },
        {
          name: "C1",
          type: "sold",
          price: 500
        },
        {
          name: "C2",
          type: "sold",
          price: 500
        },
        {
          name: "C3",
          type: "sold",
          price: 500
        },
        {
          name: "C4",
          type: "sold",
          price: 500
        },
        {
          name: "D1",
          type: "available",
          price: 400
        },
        {
          name: "D2",
          type: "available",
          price: 400
        },
        {
          name: "D3",
          type: "available",
          price: 400
        },
        {
          name: "D4",
          type: "available",
          price: 400
        },
        {
          name: "E1",
          type: "available",
          price: 300
        },
        {
          name: "E2",
          type: "available",
          price: 300
        },
        {
          name: "E3",
          type: "booked",
          price: 300
        },
        {
          name: "E4",
          type: "booked",
          price: 300
        },
        {
          name: "F1",
          type: "available",
          price: 300
        },
        {
          name: "F2",
          type: "available",
          price: 300
        },
        {
          name: "F3",
          type: "available",
          price: 300
        },
        {
          name: "F4",
          type: "available",
          price: 300
        }
      ],
    }
  },
    computed:{
      selectedSeats(){
      const st = this.seats.filter(seat=> seat.type === 'selected');
        return st;
          },
      totalCost() {
      let tc = 0;
      this.selectedSeats.forEach((seat) => {
        tc += seat.price;
        console.log('clicked',tc)
      });

      if (this.appliedCoupon !== null) {
        tc = tc - this.appliedCoupon.discount;
      }
      return tc;
    }     
      },
      methods:{
        handleClick(i){
          const clickedSeat=this.seats[i];
          if(clickedSeat.type==='sold' || clickedSeat.type==='booked'){
            alert("Seats Has already selected");

            return
          }
             if(clickedSeat.type === "available" && this.selectedSeats.length > 2){
            alert("You can not buy more than 3 seats at a time");
            return
          }
             clickedSeat.type = clickedSeat.type === 'selected' ? 'available' : 'selected';
          console.log(clickedSeat);
        }
      },
      watch:{
        couponCode(newValue) {
          this.err_msg=false;
          if(newValue.length >= 10){
            let searchedCoupons =this.coupons.filter((item)=>item.code === newValue);
          if(searchedCoupons.length === 1){
            this.appliedCoupon = searchedCoupons[0];
            this.couponCode=""
          }else{
            // alert("coupon not valid!!");
            this.err_msg=true;
          }
          }
        }
      }

}
</script>

<template>
  <HelloWorld/>
    <header>
      <h2>Vue JS 3 Bangla Tutorial</h2>
    </header>
         <div id="app">
      <h1 class="app-title">Ticket Booking App</h1>
      <div class="ticket-app">
        <div class="ticket-app__top">
             <div class="seat-states">
            <div class="seat-state" v-for="(value)  in seatStates">
              <div
                class="seat-state__color"
              :style="{'background-color':value.color}"
              ></div>
              <div class="seat-state__text">{{ value.text }}</div>
            </div>
          </div>
        </div>
        <div class="ticket-app__bottom">
          <div class="ticket-app__left">
            <div class="bus">
              <div class="bus__top">
                <div class="bus__door">Door</div>
                <div class="bus__driver">Driver</div>
              </div>
                <div class="seats">
                <div
                  class="seat"
                  :class="{
                    'seat--sold': seat.type === 'sold',
                    'seat--booked': seat.type === 'booked',
                    'seat--selected': seat.type === 'selected'
                    
                  }"
                  v-for="(seat, i) in seats"
                  :key="seat.name"
                  @click="handleClick(i)"
                >
                  {{seat.name}}
               
                </div>
              </div>
            </div>
          </div>
          <div class="ticket-app__instruction" v-if="selectedSeats.length === 0">
            no Seats Selected <br>
            Select Some Seats First
          </div>
          <div class="ticket-app__right" v-else>
            <div class="cart">
              <strong>Selected Seats</strong>
              <table class="selected-seats">
                <thead>
                  <tr>
                    <th>Seat</th>
                    <th>Price</th>
                  </tr>
                </thead>
                <tbody>
                  <tr v-for="(seat) in selectedSeats" :key="seat.name">
                    <td>{{seat.name}}</td>
                    <td>Tk. {{seat.price}}</td>
                  </tr>

                  <tr v-if="appliedCoupon !== null">
                    <td>Discount</td>
                    <td>Tk. - {{appliedCoupon.discount}}</td>
                  </tr> 
                  <tr>
                    <th>Total</th>
                    <th>Tk. {{totalCost}}</th>
                  </tr>
                </tbody>
              </table>
                  <p v-if="appliedCoupon === null">
                Have a coupon? <input type="text" placeholder="10 Digits Code" v-model="couponCode"> 
                  </p>

              <p v-else>
              Applied Coupon: {{ appliedCoupon.code }}
              </p>
              <p v-if="err_msg===true">Invalid Coupon Code!</p>
            </div>
          </div>
        </div>
      </div>
      </div>

</template>

<style>
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }
  
  body {
    font-family: sans-serif;
    line-height: 1.8;
  }
  
  header {
    background-color: #1f2c46;
    color: #41b883;
    font-size: 22px;
    padding: 22px;
  }
  
  header h2 {
    text-align: center;
  }
  
  #app {
    padding: 11px;
    font-size: 14px;
  }
  
  button {
    padding: 5px 11px;
    margin: 5px;
  }
  
  .active {
    background-color: rgb(3, 78, 97);
    color: #fff;
    border: 1px solid rgb(3, 78, 97);
  }
  
  .bold {
    font-weight: bold;
  }
  
  .card {
    width: 100px;
    height: 100px;
    border: 1px solid gray;
  }
  
  input[type="text"] {
    padding: 3px 9px;
    margin: 5px;
  }
  
  .red {
    width: 100px;
    height: 50px;
    background-color: red;
  }
  
  .green {
    width: 100px;
    height: 50px;
    background-color: green;
  }
  
  .blue {
    width: 100px;
    height: 50px;
    background-color: blue;
  }
  
  table {
    width: 100%;
    border-collapse: collapse;
  }
  
  td,
  th {
    border: 1px solid gray;
    padding: 5px;
  }
  
  ul {
    margin-left: 22px;
  }
  
  /*================= Ticket App ======================= */
  .app-title {
    text-align: center;
  }
  
  .ticket-app {
    width: 500px;
    max-width: 100%;
    /* border: 1px solid gray; */
    margin: 22px auto;
    min-height: 300px;
    padding: 11px;
    position: relative;
    border-radius: 5px;
    box-shadow: 0 0 3px 2px #dedede;
  }
  
  .ticket-app__top {
    display: flex;
  }
  
  .seat-states {
    display: flex;
    flex-wrap: wrap;
  }
  
  .seat-state {
    /* border: 1px solid gray; */
    margin: 3px;
    min-height: 5px;
    min-width: 33px;
    display: flex;
    margin-right: 22px;
    /* width: 88px; */
  }
  
  .seat-state__color {
    width: 28px;
    border: 1px solid gray;
  }
  
  .seat-state__text {
    display: flex;
    align-items: center;
    justify-content: center;
    line-height: 1.4;
    margin-left: 4px;
  }
  
  .ticket-app__bottom {
    display: flex;
    margin-top: 11px;
  }
  
  .ticket-app__left {
    width: 172px;
  }
  
  .ticket-app__instruction {
    flex: 1;
    display: flex;
    justify-content: center;
    align-items: center;
    text-align: center;
  }
  
  .ticket-app__right {
    flex: 1;
    padding-left: 22px;
  }
  
  .bus {
    border: 1px solid gray;
    padding: 5px;
  }
  
  .bus__top {
    display: flex;
    justify-content: space-between;
  }
  
  .bus__door {
    width: 66px;
    border: 1px solid gray;
    padding: 3px;
    display: flex;
    justify-content: center;
    align-items: center;
  }
  
  .bus__driver {
    width: 66px;
    border: 1px solid gray;
    padding: 3px;
    display: flex;
    justify-content: center;
    align-items: center;
  }
  
  .seats {
    display: flex;
    flex-wrap: wrap;
    /* border: 1px solid gray;
    padding: 3px; */
    margin-top: 11px;
  }
  
  .seat {
    margin: 4px;
    border: 1px solid gray;
    padding: 3px;
    min-height: 11px;
    cursor: pointer;
    font-size: 15px;
    width: 28px;
  }
  
  .seat--sold {
    background-color: red;
    color: #fff;
  }
  
  .seat--booked {
    background-color: gray;
  }
  
  .seat--selected {
    background-color: #00ff00;
  }
  
  .seat:hover {
    font-weight: bold;
  }
  
  .seat:nth-child(4n -2) {
    margin-right: 12px;
  }
  
  .seat:nth-child(4n -1) {
    margin-left: 12px;
  }
  
  .cart {
    /* border: 1px solid gray;
    padding: 5px; */
  }
  
  .info {
    margin-top: 11px;
    /* border: 1px solid gray;
    padding: 5px;
    box-sizing: border-box; */
  }
  
  button.confirm {
    margin-top: 11px;
    margin-left: 0;
    margin: 0;
  }
  
  .selected-seats th,
  .selected-seats td {
    padding: 2px 9px;
    text-align: left;
  }
  
  .ticket-app input[type="text"] {
    /* width: 100%; */
    width: 46%;
    padding: 4px 9px;
  }
  
  .confirm-button {
    border: 1px solid blue;
    background-color: #006ea2;
    color: #fff;
    cursor: pointer;
    padding: 9px 44px;
    width: 100%;
    margin: 0;
    margin-top: 11px;
  }
  
  .confirm-button__disabled {
    opacity: 0.4;
  }
  
  .confirmed-dialog {
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    z-index: 999;
    background-color: rgba(255, 255, 255, 0.9);
    text-align: center;
    padding: 33px;
  }
  
  .confirmed-dialog h3 {
    color: green;
  }
</style>
