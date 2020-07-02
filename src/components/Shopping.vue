<template>
  <div id="shopping-cart">
    <Container @drop="onDrop">
      <Draggable>
        <md-button class="md-accent md-raised" @click="showList()" id="show">
          {{ cartCount }}
        </md-button>
      </Draggable>
    </Container>
    <div id="shoppingList" class="shoppingBody">
      <div class="md-layout" v-for="(item, index) in cart" :key="index">
        <div class="md-layout-item">{{ item.name }}</div>
        <div class="md-layout-item">
          <img :src="item.image" alt />
        </div>
        <div class="md-layout-item">{{ `"$"` + item.price }}</div>
        <div class="md-layout-item">
          <md-button class="md-primary" @click="removeItem(index)">
            Remove Cart
          </md-button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { Container, Draggable } from "vue-smooth-dnd";
import { applyDrag } from "./../utils";

export default {
  name: "Shopping",
  components: { Container, Draggable },
  computed: {
    StoreCart() {
      return this.$store.getters.StoreCart;
    },
    cartCount() {
      return this.StoreCart.length;
    },
    cart() {
      //  items: generateItems(50, i => ({ id: i, data: "Draggable " + i }))
      return this.$store.getters.StoreCart.map(cartitems => {
        return this.$store.getters.products.find(itemForSale => {
          return cartitems === itemForSale.id;
        });
      });
    }
  },
  methods: {
    removeItem(index) {
      //this.items = applyDrag(this.items, dropResult);
      this.$store.dispatch("removeItem", index);
    },
    onDrop(dropResult) {
      applyDrag(this.$store.dispatch("addItem", dropResult.id));
    },
    showList() {
      var modal = document.getElementById("shoppingList");
      var btn = document.getElementById("show");
      btn.onclick = function() {
        modal.style.display = "block";
      };
      window.onclick = function(event) {
        if (event.target == modal) {
          modal.style.display = "none";
        }
      };
    }
  }
};
</script>

<style lang="scss" scoped>
#shopping-cart {
  width: 100%;
  max-height: 400px;
  border: 1px solid rgba(#000, 0.12);
  .md-layout {
    margin-top: 2%;
    width: 70%;
    background-color: rgb(255, 255, 255);
    margin: auto;
    z-index: 9999999;
    padding: 20px;
    border-bottom: 1px solid rgb(126, 126, 126);
    .md-layout-item {
      margin-top: 2%;
      img {
        width: 25%;
      }
    }
  }
  .md-span {
    text-align: right;
    width: 100%;
  }
  .shoppingBody {
    display: none;
    position: fixed;
    z-index: 9999999;
    padding-top: 25px;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
    overflow: auto;
    background-color: rgb(0, 0, 0);
    background-color: rgba(0, 0, 0, 0.4);
  }
}
</style>
