<template>
  <div id="app" class="container-fluid pt-5">
    <div class="row" v-if="isActionRequired">
      <div class="col-12">
        <h3 class="text-center">{{actionMsg}}</h3>
      </div>
    </div>
    <div class="row" v-if="!isFightStart">
      <div class="col-3">
        <h4 class="text-center">Your Stats</h4>
        <self-dimensions :dimensions="player.dimensions"></self-dimensions>
      </div>
      <div class="col-3">
        <h4 class="text-center">Your Health</h4>
        <self-health :health="player.health"></self-health>
      </div>
      <div class="col-3">
        <h4 class="text-center">Enemy Health</h4>
        <enemy-health :health="enemy.health"></enemy-health>
      </div>
      <div class="col-3">
        <h4 class="text-center">Enemy Stats</h4>
        <enemy-dimensions :dimensions="enemy.dimensions"></enemy-dimensions>
      </div>
    </div>
    <div class="row" v-else>
      <div class="col-6">
        <h4 class="text-center">Your Health</h4>
        <self-health :health="player.health"></self-health>
        <h6 class="text-center">Damage Bonus: {{player.damageBonus}}</h6>
      </div>
      <div class="col-6">
        <h4 class="text-center">Enemy Health</h4>
        <enemy-health :health="enemy.health"></enemy-health>
        <h6 class="text-center">Damage Bonus: {{enemy.damageBonus}}</h6>
      </div>
    </div>
    <div class="row">
      <div class="col-6">
        <self-ability
            :skills="player.skills"
            :fighting-name="fightingList[player.skills.fightingType].type+' ('+fightingList[player.skills.fightingType].name+')'"
            :firearm-name="firearmList[player.skills.firearmType].type+' ('+firearmList[player.skills.firearmType].name+')'">
        </self-ability>
      </div>
      <div class="col-6">
        <enemy-ability
            :skills="enemy.skills"
            :fighting-name="fightingList[enemy.skills.fightingType].type+' ('+fightingList[enemy.skills.fightingType].name+')'"
            :firearm-name="firearmList[enemy.skills.firearmType].type+' ('+firearmList[enemy.skills.firearmType].name+')'">
        </enemy-ability>
      </div>
    </div>
    <div class="row" v-if="!isFightStart">
      <div class="col-6">
        <reroll @roll="rollAttributes"></reroll>
      </div>
      <div class="col-6">
        <start @start="startGame"></start>
      </div>
    </div>
    <div class="row" v-else>
      <div class="col-3">
        <fighting :is-disabled="actionStatus[0]" @fight="actionResult[0]=true;"></fighting>
      </div>
      <div class="col-3">
        <firearm :is-disabled="actionStatus[1]" @firearm="actionResult[1]=true;"></firearm>
      </div>
      <div class="col-3">
        <first-aid :is-disabled="actionStatus[2]" @heal="actionResult[2]=true;"></first-aid>
      </div>
      <div class="col-3">
        <dodge :is-disabled="actionStatus[3]" @dodge="actionResult[3]=true;"></dodge>
      </div>
    </div>
    <div class="row" v-if="fightLog.length>0">
      <div class="col-3"></div>
      <div class="col-6">
        <log :log="fightLog"></log>
      </div>
      <div class="col-3"></div>
    </div>
  </div>
</template>

<script>
import SelfDimensions from "@/components/Stats/Self/SelfDimensions";
import SelfHealth from "@/components/Stats/Self/SelfHealth";
import SelfAbility from "@/components/Stats/Self/SelfAbility";
import EnemyDimensions from "@/components/Stats/Enemy/EnemyDimensions";
import EnemyHealth from "@/components/Stats/Enemy/EnemyHealth";
import EnemyAbility from "@/components/Stats/Enemy/EnemyAbility";
import Reroll from "@/components/Actions/Control/Reroll";
import Start from "@/components/Actions/Control/Start";
import Fighting from "@/components/Actions/Combat/Fighting";
import Firearm from "@/components/Actions/Combat/Firearm";
import FirstAid from "@/components/Actions/Combat/FirstAid";
import Dodge from "@/components/Actions/Combat/Dodge";
import Log from "@/components/Actions/Combat/Log";

export default {
  name: 'App',
  components: {
    SelfDimensions,
    SelfHealth,
    SelfAbility,
    EnemyDimensions,
    EnemyHealth,
    EnemyAbility,
    Reroll,
    Start,
    Fighting,
    Firearm,
    FirstAid,
    Dodge,
    Log,
  },
  methods: {
    randomInt(min,max) {
      return Math.floor(Math.random()*(max-min+1))+min;
    },
    rollAttributes() {
      this.player.dimensions.strength=this.randomInt(3,18) * 5;
      this.player.dimensions.condition=this.randomInt(3,18) * 5;
      this.player.dimensions.size=this.randomInt(8,18) * 5;
      this.player.dimensions.dexterity=this.randomInt(3,18) * 5;
      this.player.dimensions.appearance=this.randomInt(3,18) * 5;
      this.player.dimensions.intelligence=this.randomInt(8,18) * 5;
      this.player.dimensions.power=this.randomInt(3,18) * 5;
      this.player.dimensions.education=this.randomInt(8,18)* 5;
      this.player.dimensions.luck=this.randomInt(3,18) * 5;
      this.player.skills.fightingType=this.randomInt(1,15);
      if(this.fightingList[this.player.skills.fightingType].type==='Brawl') {
        this.player.skills.fighting=this.randomInt(25,80);
      } else if(this.fightingList[this.player.skills.fightingType].type==='Whip') {
        this.player.skills.fighting=this.randomInt(5,80);
      } else if(this.fightingList[this.player.skills.fightingType].type==='Chainsaw') {
        this.player.skills.fighting=this.randomInt(10,80);
      } else if(this.fightingList[this.player.skills.fightingType].type==='Axe') {
        this.player.skills.fighting=this.randomInt(15,80);
      } else if(this.fightingList[this.player.skills.fightingType].type==='Sword') {
        this.player.skills.fighting=this.randomInt(20,80);
      } else if(this.fightingList[this.player.skills.fightingType].type==='Garrote') {
        this.player.skills.fighting=this.randomInt(15,80);
      } else if(this.fightingList[this.player.skills.fightingType].type==='Flail') {
        this.player.skills.fighting=this.randomInt(10,80);
      } else if(this.fightingList[this.player.skills.fightingType].type==='Spear') {
        this.player.skills.fighting=this.randomInt(20,80);
      } else {
        this.player.skills.fighting=0;
      }
      this.player.skills.firearmType=this.randomInt(1,24);
      if(this.firearmList[this.player.skills.firearmType].type==='Handgun') {
        this.player.skills.firearm=this.randomInt(20,80);
      } else if(this.firearmList[this.player.skills.firearmType].type==='Bow') {
        this.player.skills.firearm=this.randomInt(15,80);
      } else if(this.firearmList[this.player.skills.firearmType].type==='Rifle/Shotgun') {
        this.player.skills.firearm=this.randomInt(25,80);
      } else if(this.firearmList[this.player.skills.firearmType].type==='Sub Machine Gun') {
        this.player.skills.firearm=this.randomInt(15,80);
      } else if(this.firearmList[this.player.skills.firearmType].type==='Machine Gun') {
        this.player.skills.firearm=this.randomInt(10,80);
      } else if(this.firearmList[this.player.skills.firearmType].type==='Heavy Weapon') {
        this.player.skills.firearm=this.randomInt(10,80);
      } else {
        this.player.skills.firearm=0;
      }
      this.player.skills.firstAid=this.randomInt(30,80);
      this.player.skills.dodge=this.randomInt(Math.floor(this.player.dimensions.dexterity/2),80);
      this.player.health.maxHealth=Math.floor((this.player.dimensions.condition+this.player.dimensions.size)/10);
      if(this.player.dimensions.strength+this.player.dimensions.condition<=64) {
        this.player.damageBonus='-2';
      } else if(this.player.dimensions.strength+this.player.dimensions.condition<=84) {
        this.player.damageBonus='-1';
      } else if(this.player.dimensions.strength+this.player.dimensions.condition<=124) {
        this.player.damageBonus='0';
      } else if(this.player.dimensions.strength+this.player.dimensions.condition<=164) {
        this.player.damageBonus='+1D4';
      } else {
        this.player.damageBonus='+1D6';
      }
      this.enemy.dimensions.strength=this.randomInt(3,18) * 5;
      this.enemy.dimensions.condition=this.randomInt(3,18) * 5;
      this.enemy.dimensions.size=this.randomInt(8,18) * 5;
      this.enemy.dimensions.dexterity=this.randomInt(3,18) * 5;
      this.enemy.dimensions.appearance=this.randomInt(3,18) * 5;
      this.enemy.dimensions.intelligence=this.randomInt(8,18) * 5;
      this.enemy.dimensions.power=this.randomInt(3,18) * 5;
      this.enemy.dimensions.education=this.randomInt(8,18)* 5;
      this.enemy.dimensions.luck=this.randomInt(3,18) * 5;
      this.enemy.skills.fightingType=this.randomInt(1,15);
      if(this.fightingList[this.enemy.skills.fightingType].type==='Brawl') {
        this.enemy.skills.fighting=this.randomInt(25,80);
      } else if(this.fightingList[this.enemy.skills.fightingType].type==='Whip') {
        this.enemy.skills.fighting=this.randomInt(5,80);
      } else if(this.fightingList[this.enemy.skills.fightingType].type==='Chainsaw') {
        this.enemy.skills.fighting=this.randomInt(10,80);
      } else if(this.fightingList[this.enemy.skills.fightingType].type==='Axe') {
        this.enemy.skills.fighting=this.randomInt(15,80);
      } else if(this.fightingList[this.enemy.skills.fightingType].type==='Sword') {
        this.enemy.skills.fighting=this.randomInt(20,80);
      } else if(this.fightingList[this.enemy.skills.fightingType].type==='Garrote') {
        this.enemy.skills.fighting=this.randomInt(15,80);
      } else if(this.fightingList[this.enemy.skills.fightingType].type==='Flail') {
        this.enemy.skills.fighting=this.randomInt(10,80);
      } else if(this.fightingList[this.enemy.skills.fightingType].type==='Spear') {
        this.enemy.skills.fighting=this.randomInt(20,80);
      } else {
        this.enemy.skills.fighting=0;
      }
      this.enemy.skills.firearmType=this.randomInt(1,24);
      if(this.firearmList[this.enemy.skills.firearmType].type==='Handgun') {
        this.enemy.skills.firearm=this.randomInt(20,80);
      } else if(this.firearmList[this.enemy.skills.firearmType].type==='Bow') {
        this.enemy.skills.firearm=this.randomInt(15,80);
      } else if(this.firearmList[this.enemy.skills.firearmType].type==='Rifle/Shotgun') {
        this.enemy.skills.firearm=this.randomInt(25,80);
      } else if(this.firearmList[this.enemy.skills.firearmType].type==='Sub Machine Gun') {
        this.enemy.skills.firearm=this.randomInt(15,80);
      } else if(this.firearmList[this.enemy.skills.firearmType].type==='Machine Gun') {
        this.enemy.skills.firearm=this.randomInt(10,80);
      } else if(this.firearmList[this.enemy.skills.firearmType].type==='Heavy Weapon') {
        this.enemy.skills.firearm=this.randomInt(10,80);
      } else {
        this.enemy.skills.firearm=0;
      }
      this.enemy.skills.firstAid=this.randomInt(30,80);
      this.enemy.skills.dodge=this.randomInt(Math.floor(this.enemy.dimensions.dexterity/2),80);
      this.enemy.health.maxHealth=Math.floor((this.enemy.dimensions.condition+this.enemy.dimensions.size)/10);
      if(this.enemy.dimensions.strength+this.enemy.dimensions.condition<=64) {
        this.enemy.damageBonus='-2';
      } else if(this.enemy.dimensions.strength+this.enemy.dimensions.condition<=84) {
        this.enemy.damageBonus='-1';
      } else if(this.enemy.dimensions.strength+this.enemy.dimensions.condition<=124) {
        this.enemy.damageBonus='0';
      } else if(this.enemy.dimensions.strength+this.enemy.dimensions.condition<=164) {
        this.enemy.damageBonus='+1D4';
      } else {
        this.enemy.damageBonus='+1D6';
      }
    },
    startGame() {
      if(this.player.health.maxHealth===0||this.enemy.health.maxHealth===0) {
        this.rollAttributes()
      }
      this.isFightStart=true;
      this.fightLog=[];
      if(this.player.dimensions.dexterity>this.enemy.dimensions.dexterity) {
        this.playerTurn();
      } else if(this.player.dimensions.dexterity<this.enemy.dimensions.dexterity) {
        this.enemyTurn();
      } else {
        if(this.player.skills.fighting+this.player.skills.firearm>this.enemy.skills.fighting+this.enemy.skills.firearm) {
          this.playerTurn();
        } else if(this.player.skills.fighting+this.player.skills.firearm<this.enemy.skills.fighting+this.enemy.skills.firearm) {
          this.enemyTurn();
        } else {
          if (this.randomInt(1, 2) === 1) {
            this.playerTurn();
          } else {
            this.enemyTurn();
          }
        }
      }
    },
    playerTurn() {
      this.actionStatus[0]=false;
      this.actionStatus[1]=false;
      if(this.player.health.lostHealth===0) {
        this.actionStatus[2]=true;
        this.actionStatus[3]=true;
        this.actionMsg="Player's turn. Please choose attack type"
        this.isActionRequired=true;
        while(!(this.actionResult[0]===true||this.actionResult[1]===true)) {
          setTimeout(this.playerTurn(), 5);
        }
        if(this.actionResult[0]===true) {
          this.actionResult[0]=false;
          let fightNumber=this.randomInt(1,100);
          let enemyResponse=this.randomInt(1,2);
          let enemyNumber=this.randomInt(1,100);
          let fightSuccess=0;
          let enemySuccess=0;
          if(fightNumber===1) {
            fightSuccess=6;
          } else if(fightNumber<=Math.floor(this.player.skills.fighting/5)) {
            fightSuccess=5;
          } else if(fightNumber<=Math.floor(this.player.skills.fighting/2)) {
            fightSuccess=4;
          } else if(fightNumber<=this.player.skills.fighting) {
            fightSuccess=3;
          } else if((this.player.skills.fighting>=50&&fightNumber===100)||(this.player.skills.fighting<50&&fightNumber>=96)) {
            fightSuccess=1;
          } else {
            fightSuccess=2;
          }
          if(enemyResponse===1) {
            if(enemyNumber===1) {
              enemySuccess=6;
            } else if(enemyNumber<=Math.floor(this.enemy.skills.fighting/5)) {
              enemySuccess=5;
            } else if(enemyNumber<=Math.floor(this.enemy.skills.fighting/2)) {
              enemySuccess=4;
            } else if(enemyNumber<=this.enemy.skills.fighting) {
              enemySuccess=3;
            } else if((this.enemy.skills.fighting>=50&&enemyNumber===100)||(this.enemy.skills.fighting<50&&enemyNumber>=96)) {
              enemySuccess=1;
            } else {
              enemySuccess=2;
            }
          } else {
            if(enemyNumber===1) {
              enemySuccess=6;
            } else if(enemyNumber<=Math.floor(this.enemy.skills.dodge/5)) {
              enemySuccess=5;
            } else if(enemyNumber<=Math.floor(this.enemy.skills.dodge/2)) {
              enemySuccess=4;
            } else if(enemyNumber<=this.enemy.skills.dodge) {
              enemySuccess=3;
            } else if((this.enemy.skills.dodge>=50&&enemyNumber===100)||(this.enemy.skills.dodge<50&&enemyNumber>=96)) {
              enemySuccess=1;
            } else {
              enemySuccess=2;
            }
          }
        } else {
          this.actionResult[1]=false;
        }
      } else {
        this.actionStatus[2]=false;
        this.actionStatus[3]=true;
        this.actionMsg="Player's turn. Please choose attack type or to heal"
        this.isActionRequired=true;
      }
    },
    enemyTurn() {

    }
  },
  data() {
    return {
      fightingList: [
        {
          name:"",
          type:"",
          damage:"",
          error:-1,
        },
        {
          name:"徒手",
          type:"Brawl",
          damage:"1D3+DB",
          error:101,
        },
        {
          name:"小型棍",
          type:"Brawl",
          damage:"1D6+DB",
          error:101,
        },
        {
          name:"大型棍/大型刀/铁棍",
          type:"Brawl",
          damage:"1D8+DB",
          error:101,
        },
        {
          name:"中型刀",
          type:"Brawl",
          damage:"1D4+2+DB",
          error:101,
        },
        {
          name:"小型刀",
          type:"Brawl",
          damage:"1D4+DB",
          error:101,
        },
        {
          name:"长鞭",
          type:"Whip",
          damage:"1D3+DB/2",
          error:101,
        },
        {
          name:"电锯",
          type:"Chainsaw",
          damage:"2D8",
          error:95,
        },
        {
          name:"绞具",
          type:"Garrote",
          damage:"1D6+DB",
          error:101,
        },
        {
          name:"双节棍",
          type:"Flail",
          damage:"1D8+DB",
          error:101,
        },
        {
          name:"斧头/镰刀",
          type:"Axe",
          damage:"1D6+1+DB",
          error:101,
        },
        {
          name:"伐木斧",
          type:"Axe",
          damage:"1D8+2+DB",
          error:101,
        },
        {
          name:"轻剑",
          type:"Sword",
          damage:"1D6+DB",
          error:101,
        },
        {
          name:"中型剑",
          type:"Sword",
          damage:"1D6+1+DB",
          error:101,
        },
        {
          name:"大型剑",
          type:"Sword",
          damage:"1D8+1+DB",
          error:101,
        },
        {
          name:"矛",
          type:"Spear",
          damage:"1D8+1",
          error:101
        },
      ],
      firearmList:[
        {
          name:"",
          type:"",
          damage:"",
          error:-1,
        },
        {
          name:"弓箭",
          type:"Bow",
          damage:"1D6+DB/2",
          error:97,
        },
        {
          name:"弩",
          type:"Bow",
          damage:"1D8+2",
          error:96,
        },
        {
          name:"燧发枪",
          type:"Handgun",
          damage:"1D6+1",
          error:95,
        },
        {
          name:".32左轮手枪",
          type:"Handgun",
          damage:"1D8",
          error:100,
        },
        {
          name:".357马格南左轮手枪",
          type:"Handgun",
          damage:"1D8+1D4",
          error:100,
        },
        {
          name:"9mm格洛克17",
          type:"Handgun",
          damage:"1D10",
          error:98,
        },
        {
          name:".45马格南左轮手枪",
          type:"Handgun",
          damage:"1D10+1D4+2",
          error:100,
        },
        {
          name:".45左轮手枪",
          type:"Handgun",
          damage:"1D10+2",
          error:100,
        },
        {
          name:"沙漠之鹰",
          type:"Handgun",
          damage:"1D10+1D6+3",
          error:100,
        },
        {
          name:".58斯普林菲尔德步枪",
          type:"Rifle/Shotgun",
          damage:"1D10+4",
          error:95,
        },
        {
          name:"SKS半自动步枪",
          type:"Rifle/Shotgun",
          damage:"2D6+1/4D6+2",
          error:97,
        },
        {
          name:".303李恩菲尔德",
          type:"Rifle/Shotgun",
          damage:"2D6+4",
          error:100,
        },
        {
          name:"20号双管霰弹枪",
          type:"Rifle/Shotgun",
          damage:"2D6/4D6/1D6/1D3/2D3",
          error:100,
        },
        {
          name:"16号双管霰弹枪",
          type:"Rifle/Shotgun",
          damage:"2D6+2/4D6+4/1D6+1/2D6+2/1D4/2D4",
          error:100,
        },
        {
          name:"12号双管霰弹枪",
          type:"Rifle/Shotgun",
          damage:"4D6/8D6/2D6/1D6",
          error:100,
        },
        {
          name:"12号SPAS",
          type:"Rifle/Shotgun",
          damage:"4D6/2D6/1D6",
          error:98,
        },
        {
          name:"AK-47",
          type:"Rifle/Shotgun",
          damage:"2D6+1/4D6+2",
          error:100,
        },
        {
          name:"M4",
          type:"Rifle/Shotgun",
          damage:"2D6/4D6",
          error:97,
        },
        {
          name:"巴雷特M70",
          type:"Rifle/Shotgun",
          damage:"2D6",
          error:99,
        },
        {
          name:"汤普森冲锋枪",
          type:"Sub Machine Gun",
          damage:"1D10+2",
          error:96,
        },
        {
          name:"乌兹微型冲锋枪",
          type:"Sub Machine Gun",
          damage:"1D10",
          error:98,
        },
        {
          name:"M1882加特林机枪",
          type:"Machine Gun",
          damage:"2D6+4",
          error:96,
        },
        {
          name:"M1918式勃朗宁自动步枪",
          type:"Machine Gun",
          damage:"2D6+4",
          error:100,
        },
        {
          name:"M79榴弹发射器",
          type:"Heavy Weapon",
          damage:"3D10",
          error:99,
        },
      ],
      player: {
        dimensions: {
          strength:0,
          condition:0,
          size:0,
          dexterity:0,
          appearance:0,
          intelligence:0,
          power:0,
          education:0,
          luck:0,
        },
        skills: {
          fighting:0,
          fightingType:0,
          firearm:0,
          firearmType:0,
          firstAid:0,
          dodge:0,
        },
        health: {
          maxHealth:0,
          lostHealth:0,
        },
        damageBonus:"",
      },
      enemy:{
        dimensions: {
          strength:0,
          condition:0,
          size:0,
          dexterity:0,
          appearance:0,
          intelligence:0,
          power:0,
          education:0,
          luck:0,
        },
        skills: {
          fighting:0,
          fightingType:0,
          firearm:0,
          firearmType:0,
          firstAid:0,
          dodge:0,
        },
        health: {
          maxHealth:0,
          lostHealth:0,
        },
        damageBonus:"",
      },
      isFightStart:false,
      fightLog: [],
      actionStatus: [
          false,
          false,
          false,
          false,
      ],
      actionResult: [
        false,
        false,
        false,
        false
      ],
      isActionRequired:false,
      actionMsg:'',
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
button {
  font-size: 20px;
  background-color: #eee;
  padding: 12px;
  margin: 10px;
}
</style>
