<template>
  <div class="container">
    <h2>Cake creation</h2>
    <p class="m-0">Bisquit price: 0.5 UAH for 1 mm</p>
    <p class="m-0">Beze price: 0.8 UAH for 1 mm</p>
    <p class="m-0">Cherry price: 1 UAH for 1 mm</p>
    <hr>
    <h2>Height of cake: {{heightCounter}} mm</h2>
    <transition name="fade">
      <div class="alert alert-danger alert-dismissible fade show" role="alert" v-show="isExceed">
        <h2>Maximum height is 80 mm!</h2>
        <button @click="isExceed = false" type="button" class="close" data-dismiss="alert" aria-label="Close">
          <span aria-hidden="true">&times;</span>
        </button>
      </div>
    </transition>
    <div class="cake-wrap">
      <div class="cake">
        <div v-for="layer in layers"
            :key="layer.id"
            :id="layer.id"
            :class="layer.layerClass"
            :style="{height: layer.height*4 + 'px',background: layer.color}"
            class="layer"
        ></div>
      </div>
      <div class="layers-set">
        <table class="table">
          <thead>
            <tr>
              <td>Type</td>
              <td>Height</td>
              <td align="center">Color</td>
              <td align="center">Actions</td>
            </tr>
          </thead>
          <tbody>
            <tr>
              <td>Bisquit</td>
              <td>
                <div class="input-group">
                  <input type="text"
                    class="form-control"
                    v-model.number="layerConf.bisquit.height">
                  <div class="input-group-append">
                    <span class="input-group-text counters-wrap">
                      <span class="rotateY-90" @click="incLayerHeight('bisquit')">&lt;</span>
                      <span class="rotateY-90" @click="dicLayerHeight('bisquit')">&gt;</span>
                    </span>
                    <span class="input-group-text">mm</span>
                  </div>
                </div>
                </td>
              <td align="center">
                <select v-model="layerConf.bisquit.color">
                  <option value="#775316">brown</option>
                  <option value="tomato">tomato</option>
                  <option value="#000000">black</option>
                  <option value="#40d86b">green</option>
                </select>
              </td>
              <td align="center">
                <button class="btn btn-success" @click="addLayer('bisquit')">+</button>
                <button class="btn btn-danger" @click="removeLayer('bisquit')">x</button>
              </td>
            </tr>
            <tr>
              <td>Beze</td>
              <td>
                <div class="input-group">
                  <input type="text"
                    class="form-control"
                    v-model.number="layerConf.beze.height">
                  <div class="input-group-append">
                    <span class="input-group-text counters-wrap">
                      <span class="rotateY-90" @click="incLayerHeight('beze')">&lt;</span>
                      <span class="rotateY-90" @click="dicLayerHeight('beze')">&gt;</span>
                    </span>
                    <span class="input-group-text">mm</span>
                  </div>
                </div>
              </td>
              <td align="center">
                <select v-model="layerConf.beze.color">
                  <option value="#775316">brown</option>
                  <option value="tomato">tomato</option>
                  <option value="#000000">black</option>
                  <option value="#40d86b">green</option>
                </select>
              </td>
              <td align="center">
                <button class="btn btn-success" @click="addLayer('beze')">+</button>
                <button class="btn btn-danger" @click="removeLayer('beze')">x</button>
              </td>
            </tr>
            <tr>
              <td>Cherry</td>
              <td>
                <div class="input-group">
                  <input type="text"
                    class="form-control"
                    v-model.number="layerConf.cherry.height">
                  <div class="input-group-append">
                    <span class="input-group-text counters-wrap">
                      <span class="rotateY-90" @click="incLayerHeight('cherry')">&lt;</span>
                      <span class="rotateY-90" @click="dicLayerHeight('cherry')">&gt;</span>
                    </span>
                    <span class="input-group-text">mm</span>
                  </div>
                </div>
              </td>
              <td align="center">
                <select v-model="layerConf.cherry.color">
                  <option value="#775316">brown</option>
                  <option value="tomato">tomato</option>
                  <option value="#000000">black</option>
                  <option value="#40d86b">green</option>
                </select>
              </td>
              <td align="center">
                <button class="btn btn-success" @click="addLayer('cherry')">+</button>
                <button class="btn btn-danger" @click="removeLayer('cherry')">x</button>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
    <div class="d-flex justify-content-between align-items-center mt-2">
      <div class="d-flex flex-column">
        <h2 class="h2-amount-of-cakes d-flex align-items-center justify-content-between">
          <span class="mr-3">Amount of cakes: {{amountOfCakes}}</span>
          <div class="input-group">
            <input type="text" 
              class="form-control amount" 
              v-model.number="amountOfCakes">
            <div class="input-group-append">
              <span class="input-group-text counters-wrap">
                <span class="rotateY-90" @click="amountOfCakes++">&lt;</span>
                <span class="rotateY-90" @click="amountOfCakes--">&gt;</span>
              </span>
            </div>
          </div>
        </h2>
        <h2 class="mt-2">Total sum: {{getSum()}} UAH</h2>
      </div>
      <button class="btn btn-primary float-right m-2" @click="reset">Reset</button>
    </div>
    <div class="d-flex justify-content-center mt-4">
      <button class="btn btn-success btn-lg">Order now!</button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      layers: [],
      currentId: 0,
      sum: 0,
      heightCounter: 0,
      isExceed: false,
      amountOfCakes: 1,
      layerConf: {
        bisquit: {
          height: 10,
          price: 0.5,
          color: "#775316",
        },
        beze: {
          height: 5,
          price: 0.8,
          color: "#40d86b"
        },
        cherry: {
          height: 5,
          price: 1,
          color: "tomato"
        }
      }
    };
  },
  methods: {
    reset() {
      this.layers = [];
      this.currentId = 0;
      this.sum = 0;
      this.heightCounter = 0;
      this.isExceed = false;
      this.amountOfCakes = 1;
    },
    addLayer(layer) {
      if (this.heightCounter + this.layerConf[layer].height >= 81) {
        this.isExceed = true;
        return;
      }
      this.layers.push({
        price: this.layerConf[layer].price,
        height: this.layerConf[layer].height,
        color: this.layerConf[layer].color,
        layerClass: layer,
        id: this.currentId++
      });
      this.heightCounter += this.layerConf[layer].height;
      this.sum =
      this.sum + this.layerConf[layer].price * this.layerConf[layer].height;
      this.isExceed = false;
    },
    removeLayer(layer) {
      for (let i = this.layers.length - 1; i >= 0; i--) {
        if (this.layers[i].layerClass == layer) {
          this.layers.splice(this.layers[i].id, 1, "empty");
          this.sum =
            this.sum -
            this.layerConf[layer].price * this.layerConf[layer].height;
          this.heightCounter -= this.layerConf[layer].height;
          return;
        }
      }
    },
    incLayerHeight(layer){
      this.layerConf[layer].height++;
    },
    dicLayerHeight(layer){
      this.layerConf[layer].height--;
    },
    getSum() {
      return (this.sum * this.amountOfCakes).toFixed(1);
    }
  }
};
</script>

<style>
.fade-enter-active, .fade-leave-active {
  transition: opacity .5s;
}
.fade-enter, .fade-leave-to /* .fade-leave-active до версии 2.1.8 */ {
  opacity: 0;
}
.form-control.amount {
  max-width: 60px;
}
.h2-amount-of-cakes {
  min-width: 400px;
}
.h2-amount-of-cakes .input-group {
  width: auto;
}
.counters-wrap {
  width: 30px;
  height: 38px;
  position: relative;
}
.rotateY-90 {
  transform: rotate(90deg);
  cursor: pointer;
  text-align: center;
  position: absolute;
  user-select: none;
}
.rotateY-90:first-child {
  top: -6px;
  width: 22px;
  left: 3px;
  height: 28px;
  border-right: 1px solid #ced4da;
}
.rotateY-90:last-child {
  bottom: -6px;
  width: 16px;
  left: 6px;
  height: 27px;
}
.layer {
  width: 100%;
  text-align: center;
}
.bisquit {
  background: #775316;
}
.beze {
  background: #40d86b;
}
.cherry {
  background: tomato;
}
.cake-wrap {
  background: #fff6f6;
  height: 320px;
  display: flex;
  justify-content: space-between;
}
.cake {
  display: inline-block;
  height: 100%;
  width: 40%;
  display: flex;
  align-items: center;
  flex-direction: column;
  justify-content: center;
  padding: 10px;
}
.layers-set {
  display: inline-block;
  height: 100%;
  width: 50%;
  display: flex;
  align-items: center;
  padding-right: 10px;
}
.table {
  border: 1px solid #ccc;
}
.table td {
  border: 1px solid #ccc;
}
.table input {
  width: 60px !important;
  margin-right: 10px;
}
</style>
