/* eslint-disable */
<template>
  <div class="container">
    <div class="row">
      <div
        v-for="k in kuponlar"
        :key="k.id"
        class="col-md-3"
        style="border: 1px solid green"
      >
        <h2>{{ k.name }}</h2>
        <p>{{ k.desc }}</p>
        <select v-model="selectedRule">
          <option v-for="r in rules" :key="r.id" :value="r">
            {{ r.name }}
          </option>
        </select>
        {{ carp(k.oranlar) > 1 ? carp(k.oranlar) : 0 }}
        {{ k.oranlar }}
        <!-- <strong v-for="t in k.oranlar">{{ carp(t) }}</strong> -->
      </div>
    </div>

    <div class="row">
      <div class="col-md-6">
        <div v-for="e in events" :key="e.id">
          {{ e.matchName }}
          <span v-for="o in e.oranlar" :key="o" @click="modaliAc(o)">
            {{ o }}
          </span>
        </div>
      </div>
    </div>
    <b-modal v-if="secilenOran" v-model="modal" :title="secilenOran" @ok="ekle">
      {{ selectedKupon }}
      <select v-model="selectedKupon">
        <option v-for="k in kuponlar" :key="k.id" :value="k">
          {{ k.name }}
        </option>
      </select>
    </b-modal>
  </div>
</template>

<script>
export default {
  data: () => ({
    secilenOran: null,
    selectedKupon: null,
    selectedRule: null,
    modal: false,
    kuponlar: [
      {
        name: 'Banko',
        id: 1,
        desc: 'buraya 2 ustu gelemez',
        oranlar: []
      },
      {
        name: 'Supriz',
        id: 2,
        desc: '3 un alti gelemez',
        oranlar: []
      }
    ],
    rules: [
      {
        name: '2 den kucuk',
        id: 1
      },
      {
        name: '3 den buyuk',
        id: 2
      }
    ],
    events: [
      {
        matchName: 'Sivas - Galatasaray',
        teamId: 1,
        oranlar: ['1.95', '2.80', '3.05']
      },
      {
        matchName: 'Besiktas - Ankaragucu',
        teamId: 1,
        oranlar: ['2', '2.80', '3.05']
      }
    ]
  }),

  methods: {
    carp(oranlar) {
      return oranlar.reduce(
        (acc, item) => (acc * Math.round(item * 100)) / 100,
        1
      )
    },
    ekle() {
      this.kuponlar.map((item) => {
        return item.id === this.selectedKupon.id
          ? item.oranlar.push(this.secilenOran)
          : false
      })
    },

    modaliAc(item) {
      this.modal = !this.modal
      this.secilenOran = item
    }
  }
}
</script>
