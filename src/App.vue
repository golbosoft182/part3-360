<template>
  <div class="container mt-5">
    <div class="row justify-content-center">
      <div class="col-md-7">
        <h2>What do you want to do for a sport activity today?</h2>
        <hr />
        <form id="sportsForm" class="mb-3">
          <div v-for="sport in sports" :key="sport.name" class="form-check form-check-inline">
            <input class="form-check-input" type="radio" v-model="selectedSport" :id="`${sport.name}-radio`" :value="sport.name" name="sport">
            <label class="form-check-label" :for="`${sport.name}-radio`">{{ sport.name }}</label>
          </div>
        </form>
        <div id="sportBoxes" class="mb-3 p-3 sport-boxes" :style="{ backgroundColor: selectedSportColor }">
          <div class="cstm-col-first">
            <div :id="'Football-box'" class="sport-box">Football</div>
          </div>
          <div class="cstm-col-right">
            <div :id="'Volleyball-box'" class="sport-box">Volleyball</div>
          </div>
          <div class="cstm-col-bottom">
            <div :id="'Badminton-box'" class="sport-box">Badminton</div>
          </div>
          <div class="me-box" v-if="showMeBox" :style="meBoxStyle">Me</div>
        </div>
        <div class="result">
          <strong>Select Sport:</strong>&nbsp;<span id="resultSport">{{ selectedSport }}</span>
        </div>
        <hr />
        <strong>Equipment:</strong>
        <ul id="equipmentList">
          <li v-for="equipment in selectedSportEquipments" :key="equipment">{{ equipment }}</li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      sports: [
        { name: 'Football', bg_color: '#6bb46e', equipments: ['Ball', 'Football Shoes', 'Goalpost'] },
        { name: 'Volleyball', bg_color: '#c30f42', equipments: ['Ball', 'Nets'] },
        { name: 'Badminton', bg_color: '#2a82be', equipments: ['Shuttlecock', 'Nets', 'Racket'] }
      ],
      selectedSport: '',
      showMeBox: false,
      meBoxStyle: {
        top: '50%',
        left: '50%',
        transform: 'translate(-50%, -50%)',
        width: '150px',
        height: '85px',
        textAlign: 'center',
        lineHeight: '85px',
        color: '#ffffff',
        backgroundColor: 'grey',
        zIndex: 1,
      },
    };
  },
  computed: {
    selectedSportColor() {
      const selectedSportObj = this.sports.find(sport => sport.name === this.selectedSport);
      return selectedSportObj ? selectedSportObj.bg_color : '';
    },
    selectedSportEquipments() {
      const selectedSportObj = this.sports.find(sport => sport.name === this.selectedSport);
      return selectedSportObj ? selectedSportObj.equipments : [];
    }
  },
  methods: {
    moveMeBox() {
      if (this.selectedSport) {
        const meBox = document.querySelector('.me-box');
        const targetBox = document.querySelector(`#${this.selectedSport}-box`);

        if (meBox && targetBox) {
          meBox.style.transition = 'all 0.5s ease-in-out';

          const targetPosition = targetBox.getBoundingClientRect();
          meBox.style.transform = `translate(${targetPosition.left}px, ${targetPosition.top}px)`;
          meBox.style.opacity = 0;

          setTimeout(() => {
            targetBox.appendChild(meBox);
            meBox.style.transform = 'translate(0, 0)';
            meBox.style.opacity = 1;
          }, 500);
        }
      }
    }
  },
  watch: {
    selectedSport() {
      if (!this.selectedSport) {
        this.showMeBox = false;
      } else {
        this.showMeBox = true;
        this.moveMeBox();
      }
    },
  },
  created() {
    this.showMeBox = true; 
  },
};
</script>

<style>
  .sport-box {
    width: 150px;
    height: 85px;
    text-align: center;
    line-height: 85px;
    border: 1px solid #000;
    background-color: #ffffff;
    color: #000;
    position: relative;
  }
  .me-box {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    width: 150px;
    height: 85px;
    text-align: center;
    line-height: 85px;
    color: #ffffff;
    background-color: grey;
    z-index: 1;
  }
  .sport-box .me-box {
    top: 0px !important;
    left: 0px !important;
    height: 83px;
    width: 148px;
    line-height: 83px;
    transform: translate(0%, 0%);
  }
  .sport-boxes {
    position: relative;
    border: 1px solid #000;
    min-height: 60vh;
  }
  .cstm-col-first {
    position: absolute;
    top: 15px;
    left: 15px;
  }
  .cstm-col-right {
    position: absolute;
    top: 15px;
    right: 15px;
  }
  .cstm-col-bottom {
    position: absolute;
    bottom: 15px;
    left: 50%;
    transform: translateX(-50%);
  }
</style>
