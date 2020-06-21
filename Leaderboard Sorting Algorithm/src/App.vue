<template>

    <div class="board">
          <div class="boardContainer">
            <div class="boardTitle">Leader Board</div>
                <div class="points" >
                      <div class="RankList" >
                          <div v-bind:key="number" class="Ranks" v-for="number in RankList">{{number}}</div>
                      </div>
                      <div class = "topList">
                         <UserPoints v-bind:key="topUser.name" v-for="topUser in SortedBoard.leaderBoard" :name="topUser.name" :points = "topUser.points" position = "first" />
                      </div>
                </div>
          </div>

          <RankPosition id = "Rank" :rank = "SortedBoard.userRank" :points = "users[id].points" :position = "SortedBoard.userPosition"/>

          <div class="boardContainer" >
            <div class="boardTitle">In front of you</div>
                  <div class="points">
                    <div class="RankList" >
                      <div v-bind:key="number" class="Ranks" v-for="number in SortedBoard.midRank" style="background: #28847B">{{number}}</div>
                    </div>

                   <div class = "topList">
                   <UserPoints v-bind:key="bottomUser.name" v-for="bottomUser in SortedBoard.nearUser" :name="bottomUser.name" :points = "bottomUser.points" position = "middle" />
                   </div>
              </div>
              <div class="container">
                    <div id = "spacer"></div>
                    <div class="boardTitle">Behind you</div>
                    <div class="points">
                        <div class="RankList" >
                            <div  class="Ranks" style="background: #808080">{{SortedBoard.lastRank}}</div>
                        </div>
                        <div class = "topList">
                            <UserPoints  :name="SortedBoard.closestThreat.name" :points = "SortedBoard.closestThreat.points" position = "last" />
                        </div>
                    </div>
              </div>
          </div>
    </div>
</template>

<script>
  import UserPoints from './components/UserPoints.vue'
  import RankPosition from "./components/RankPosition"

  export default {
    name: 'App',
    components: {
      RankPosition,
      UserPoints,
    },
    data: function() {
      return {
        users: [  //Example users listed in alphabetical order to demonstrate sorting. In real application users are pulled from the database
          {name: "Abraham", points: 1024, id: 0},
          {name: "Benjamin", points: 3000, id: 1},
          {name: "Carl", points: 467, id: 2},
          {name: "Diana", points: 2998, id: 3},
          {name: "Emmet", points: 672, id: 4},
          {name: "Fred", points: 4096, id: 5},
          {name: "Grant", points: 2038, id: 6},
          {name: "Haily", points: 536, id: 7},
          {name: "You", points: 2000, id: 8},     /** Change the points here to see how they change the sorted list **/
        ],
        Base: {name: "No one", points: 0, position: "last"},
        RankList:[ 1, 2, 3, 4, 5],
        id: 8
      };
    },
    computed:{
      SortedBoard: function() {
        let Leaderboard = [this.Base], NearUser = [this.Base], ClosestThreat = this.Base, rank = 1, position = "", midrank=[0], lastrank = 0; //initializing arrays
        for( let count = 0; count < this.users.length; count++){

          if(this.users[count].points > this.users[this.id].points){rank++;}

          for(let boardCount = 0; boardCount < Leaderboard.length; boardCount++){ //Leader board
            if(this.users[count].points > Leaderboard[boardCount].points){
              Leaderboard.splice(boardCount, 0, this.users[count]);
              break;
            }
          }

          if(Leaderboard.length > 5){ //If LeaderBoard gets too long remove the lowest person
            Leaderboard.splice(5,1);
          }

          if(NearUser[0].name === "No one" && this.users[count].points > this.users[this.id].points){
            NearUser = null;
            NearUser = [this.users[count]];
          }else {
            for (let midCount = 0; midCount < NearUser.length; midCount++) {
              if (this.users[count].points > this.users[this.id].points) {
                if (NearUser.length < 3) {
                  if (this.users[count].points < NearUser[midCount].points) {
                    if (NearUser[midCount + 1] != null) {
                      if (this.users[count].points < NearUser[midCount + 1].points) {
                        NearUser.splice(midCount + 2, 0, this.users[count]);
                        break;
                      } else {
                        NearUser.splice(midCount + 1, 0, this.users[count]);
                        break;
                      }
                    } else {
                      NearUser.splice(midCount + 1, 0, this.users[count]);
                      break;
                    }
                  } else {
                    if (this.users[count].points > NearUser[midCount].points) {
                      NearUser.splice(midCount, 0, this.users[count]);
                      break;
                    }
                  }
                } else if (midCount === 0) {
                  if (this.users[count].points < NearUser[0].points && this.users[count].points > NearUser[1].points) {
                    //if (this.users[count].points < NearUser[midCount].points) {
                    NearUser.splice(midCount + 1, 0, this.users[count]);
                    break;
                    //}
                  }
                } else if ((midCount + 1) < NearUser.length) {
                  if (this.users[count].points < NearUser[midCount].points && this.users[count].points > NearUser[midCount + 1].points) {
                    // if (this.users[count].points < NearUser[midCount].points) {
                    NearUser.splice(midCount + 1, 0, this.users[count]);
                    break;
                    // }
                  }
                }else {
                  if(this.users[count].points < NearUser[midCount].points){
                    NearUser.splice(midCount+1, 0, this.users[count]);
                    break;
                  }
                }
              }
            }
          }

          if(NearUser.length > 3){
            NearUser.splice(0, 1);
          }


          if( this.users[count].points <this.users[this.id].points && this.users[count].points > ClosestThreat.points){
            ClosestThreat = null;
            ClosestThreat = this.users[count];
          }

        }

          if(rank <= 5){position = "first";}
          else if(ClosestThreat.name === "No one") {position = "last";}
          else{position = "middle";}


        for(let rankCount = 0; rankCount < NearUser.length; rankCount++){
          midrank.splice(rankCount, 0, rank - (NearUser.length - rankCount));
          if(midrank[rankCount+1] === 0){midrank.splice(rankCount + 1, 1);}
        }


        if(ClosestThreat.name !== "No one") {
          lastrank = (rank + 1);
        }else{
          lastrank = rank;
        }

        console.log("user position is" + position);

        return{
          leaderBoard: Leaderboard,
          nearUser: NearUser,
          closestThreat: ClosestThreat,
          userRank: rank,
          userPosition: position,
          midRank: midrank,
          lastRank: lastrank
        }
      }
    }
  }
</script>

<style >
  #app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
  }
  #image{
    width: 100px;
    height: 100px;
  }
  .boardContainer{
    margin-left: auto;
    margin-right:auto;
    min-width: 360px;
    width:75%;
      max-width: 1000px;
    padding-right: 5px;
    padding-left: 5px;
    height: fit-content;
    padding-bottom: 20px;
    padding-top: 20px;
    border-radius: 10px;
    background: #CCE8D1;
    margin-top: 15px;
    margin-bottom: 15px;
  }
  .points{
    padding: 2.5px;
  }
  #Rank{
    margin-top: 20px;
    margin-bottom: 60px;
    margin-left: auto;
    margin-right: auto;
    height: 158px;
    width: 400px;
  }
  .boardTitle{
    font-family: Verdana;
      text-align: center;
    margin-left: auto;
    margin-right: auto;
    margin-bottom: 5px;
    font-weight: bold;
    font-size: 24px;
    color: #2c3e50;
    border-bottom-style: solid;
    border-bottom-width: 2px;
    border-bottom-color: #2c3e50;
    width: 60%;
    padding: 2px;
  }

  .RankList{
    margin-left: 15px;
    width: 50px;
    height: 100%;
    float: left;
  }

  .topList{

    margin-left: 90px;
    width: auto;
    margin-right: 25px;
  }

  .Ranks{
    font-family: Verdana;
    font-size: 24px;
    color: white;
     text-align: center;
      padding-top: 15px;
    margin-bottom: 10px;
    margin-left: 25%;
    min-width: 60px;
      height: 45px;
    width: fit-content;
    border-radius: 15%;
    background: #E06E3C;
  }

  #spacer{
    margin-top: 10px;
  }

</style>
