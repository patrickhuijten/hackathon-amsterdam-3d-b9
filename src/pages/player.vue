<template>
  <div id="player">
    <div class="player-container row justify-content-center">
      <div id="gameContainer" />
      <div class="ui-container">
        <!-- choice container -->
        <div id="choice" class="card bg-dark text-light rounded" v-if="sceneIndex == 2">
          <div class="card-title">
            <h3>Which would you prefer?</h3>
            <div class="row justify-content-between pt-2 px-5" v-if="hotspots[hotspotIndex] != null">
              <div
                class="card btn-outline-primary"
                style="width:32%; height:130px; overflow:hidden"
                v-for="(option, index) in hotspots[hotspotIndex].options"
                :key="index"
                @click=" GoToHotspot(hotspots[hotspotIndex + 1]) "
              >
                <img class="card-img" :src="option.image">
                <div class="card-img-overlay">
                  <h5
                    class="card-title bg-light rounded text-dark"
                    style="width: 110%; transform:Translate(-10px, 75px)"
                  >{{option.title}}</h5>
                </div>
              </div>
            </div>
          </div>
        </div>

        <!-- thank you -->
                <div id="thankyou" class="card bg-dark text-light rounded" v-if="sceneIndex == 3">
          <div class="card-title">
            <h3>Thank you for your participation</h3>
          </div>
          <div class="card-body display-5">
              <p>
                  Onwards and Upwards
We believe in “by the people for the people.” We are happy to have joined forces with you in co-creating our ultimate shared environment. Your contribution has enriched the conversation, so thank you! & See you soon.
</p>
          </div>
        </div>

        <div id="dashboard" class="card bg-dark text-light rounded" v-if="sceneIndex == 1">
          <div class="card-title border-bottom border-dark pt-3">
            <h2>Hondsrugpark | B9</h2>
          </div>
          <div class="card-body p-0 h-75">
            <!-- section 1 -->
            <div class="section col w-100 h-100" v-if="dashboardIndex == 0">
              <p>This is the current situation.</p>
              <h3>Area Features</h3>

              <ul class="list-group text-right text-capitalize">
                <li class="list-group-item bg-dark h5">
                  <span>1 Metro Station</span>
                  <i class="fas fa-subway ml-3"></i>
                </li>
                <li class="list-group-item bg-dark h5">
                  <span>1 Train Station</span>
                  <i class="fas fa-train ml-3"></i>
                </li>
                <li class="list-group-item bg-dark h5">
                  <span>1 Hospital</span>
                  <i class="fas fa-hospital ml-3"></i>
                </li>
              </ul>

              <button
                class="next-button btn btn-success text-center"
                @click="GoToDashboardIndex(dashboardIndex + 1); "
              >Next</button>
            </div>
            <!-- section 2 -->
            <div class="section col w-100 h-100" v-if="dashboardIndex == 1">
              <h4>Current Area Plans</h4>
              <ul class="list-group">
                <li
                  class="active list-group-item bg-dark row px-0 pt-2"
                  @click="GoToDashboardIndex(dashboardIndex + 1); LoadMap('Park,true');  LoadMap('ParkHotspots,true');GoToHotspot(hotspots[1]); "
                >
                  <h6>Build Park For The People</h6>
                  <img
                    src="../images/hondsrugweg-high-level-resize.png"
                    class="img-fluid"
                  >
                </li>
                <li class="inactive list-group-item bg-dark row p-0">
                  <h6 class="mt-2">Road Demolition</h6>
                  <img
                    src="data:image/jpeg;base64,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"
                    class="img-fluid"
                  >
                </li>
              </ul>
            </div>
            <!-- section 3 -->
            <div class="section col w-100 h-100" v-if="dashboardIndex == 2">
              <img
                src="../images/hondsrugweg-high-level-resize.png"
                class="img-fluid"
              >
              <hr class="border-light">
              <h3>Hondsrugpark</h3>
              <hr class="border-light">

              <p>
We love our park. Now we want to play in it! Do you want a gym? A skate park? A tree? Visualise and vote on your favourite amenities to be placed around the park!</p>              <p>You'll be given 4 options to place on the proposed ammenity location.</p>

              <button
                class="next-button btn btn-success text-center"
                @click=" GoToHotspot(hotspots[2]);GoToSceneIndex(2); "
              >Start The Tour</button>
            </div>

            <!-- dev tools -->
            <div class="dev" v-if="false">
              <div class="col w-100">
                <div
                  class="row justify-content-center px-3 py-1"
                  v-for="(hotspot, index) in hotspots"
                  :key="index"
                >
                  <button
                    class="btn w-100 btn-outline-warning"
                    @click="GoToHotspot(hotspot)"
                  >{{hotspot.title}}</button>
                </div>
              </div>
              <div class="col w-100">
                <div class="row justify-content-between px-3 py-1">
                  <div
                    class="option card bg-warning text-dark"
                    v-for="(option, index2) in hotspots[hotspotIndex].options"
                    :key="index2"
                  >
                    <div class="card-title m-0">{{option.title}}</div>
                    <div class="card-body p-1">
                      <i class="h1" :class="option.icon"/>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
        <div
          id="welcome"
          class="card bg-dark text-light rounded"
          style="opacity:0.9"
          v-if="sceneIndex == 0"
        >
          <img
            class="card-img"
            src="../images/hondsrugweg-high-level-resize.png"
            style="opacity:0.1"
          >

          <div class="card-body p-0">
            <div class="card-img-overlay row justify-content-start">
              <div class="col text-right">
                <h2 class="card-title"><b>Hondsrug Park</b> | B9</h2>
                <p
                  class="card-text"
 >
<b>Hondsrugpark</b> is the new Freezone in Amsterdam Zuidoost, where the City is promoting co-creation of  the area by local businesses, city officials, researchers,  current and prospective residents.

<b>B9</b>   is an interactive platform which visualises the neighbourhood in 3D, and allows for collaboration and deliberation on issues in the area. B9 is a rich, dynamic environment allowing area development plans and visions to be communicated and negotiated effectively by all stakeholders.

The establishment of the *Hondsrugpark* is the first successful large scale project to be facilitated by <b>B9</b>                <p class="card-text">
                  <b>We would like to know your thoughts on the new plan.</b>
                </p>
                <button
                  class="start-button btn btn-success"
                  @click="GoToSceneIndex(1); GoToHotspot(hotspots[0]); LoadMap('Buildings,true');"
                >Start</button>
              </div>
              <div class="col-4 py-3">
                <div class="row justify-content-center mb-3">
                  <img src="../images/ams-icon.png" class="img w-75 h-75">
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<style>
#thankyou,
#welcome {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: Translate(-50%, -50%);
min-height: 400px;
  width: 800px;
}

#dashboard {
  position: absolute;
  top: 10px;
  right: 10px;
  height: calc(100% - 20px);
  width: 300px;
}
#choice {
  position: absolute;
  bottom: 20px;
  left: 50%;
  transform: translateX(-50%);
  width: 800px;
  height: 200px;
}
.player-container {
  height: 680px;
}

#gameContainer {
  width: 100%;
  height: 100%;
}

.ui-container {
  transform: TranslateY(-100%);
  display: relative;
  width: 100%;
  height: 100%;
}
.option {
  width: calc(50% - 4px);
  margin-bottom: 5%;
}
.start-button {
  position: absolute;
  height: 40px;
  width: 200px;
  bottom: 0;
  right: 20px;
}
.next-button {
  position: absolute;
  height: 40px;
  width: calc(100% - 20px);
  bottom: 10px;
  right: 50%;
  transform: TranslateX(50%);
}
.inactive {
  opacity: 0.1;
}

.active:hover {
  filter: brightness(1.2);
}
</style>
<script>
export default {
  data() {
    return {
      id: 0,
      active: true,
      gameInstance: null,
      sceneIndex: 0,
      dashboardIndex: 0,
      hotspotIndex: 0,
      hotspots: [
        {
          id: 0,
          title: "Overview",
          position: "0,0,365",
          rotation: "36,2,0",
          zoom: "900",
          options: []
        },
        {
          id: 1,
          title: "Hondsrugpark",
          position: "-55,0,455",
          rotation: "37,-44,0",
          zoom: "350",
          options: []
        },
        {
          id: 2,
          title: "Location 1",
          position: "-52,0,480",
          rotation: "33,-4,0",
          zoom: "100",
          options: [
            {
              id: 0,
              title: "Skatepark1",
              image: "./src/images/skatepark.jpg",
              icon: "fas fa-coffee"
            },
            {
              id: 1,
              title: "Streetlight",
              image: "./src/images/streetlight.jpg",
              icon: "fas fa-leaf"
            },
            {
              id: 2,
              title: "Bench",
              image: "./src/images/bench.jpg",
              icon: "fas fa-paw"
            }
          ]
        },
  {
          id: 3,
          title: "Location 2",
          position: "-200,0,618",
          rotation: "30,-51,0",
          zoom: "100",
          options: [
            {
              id: 0,
              title: "Skatepark2",
              image: "./src/images/skatepark.jpg",
              icon: "fas fa-coffee"
            },
            {
              id: 1,
              title: "Streetlight",
              image: "./src/images/streetlight.jpg",
              icon: "fas fa-leaf"
            },
            {
              id: 2,
              title: "Bench",
              image: "./src/images/bench.jpg",
              icon: "fas fa-paw"
            }
          ]
        },
          {
          id: 4,
          title: "Location 3",
          position: "-233,0,746",
          rotation: "31,2,0",
          zoom: "100",
          options: [
            {
              id: 0,
              title: "Skatepark3",
              image: "./src/images/skatepark.jpg",
              icon: "fas fa-coffee"
            },
            {
              id: 1,
              title: "Streetlight",
              image: "./src/images/streetlight.jpg",
              icon: "fas fa-leaf"
            },
            {
              id: 2,
              title: "Bench",
              image: "./src/images/bench.jpg",
              icon: "fas fa-paw"
            }
          ]
        },
          {
          id: 5,
          title: "End",
          position: "500,0,200",
          rotation: "60,60,0",
          zoom: "50",
          options: [ 
          ]
        }
      ]
    };
  },
  computed:
  {
      nextHotspotIndex()
      {
          let i = null;
          if(this.hotspotIndex < this.hotspots.length-1)
          {
              i = this.hotspotIndex + 1;
          }
          return i;
      }
  },
  methods: {
    GoToDashboardIndex(index) {
      this.dashboardIndex = index;
    },
    GoToSceneIndex(index) {
      this.sceneIndex = index;
    },
    GoToHotspot(hotspot) {
     if(hotspot.id < 5)
     {
   
      if (this.gameInstance != null) {
          if(hotspot != null)
          {
        this.gameInstance.SendMessage(
          "CameraController",
          "SetPositionTarget",
          hotspot.position
        );
        this.gameInstance.SendMessage(
          "CameraController",
          "SetRotationTarget",
          hotspot.rotation
        );
        this.gameInstance.SendMessage(
          "CameraController",
          "SetZoomTarget",
          hotspot.zoom
        );
                      this.hotspotIndex = hotspot.id;

          }
          else
          {
              this.sceneIndex = 2;
          }
        }
      console.log(hotspot.position);
     }
     else
     {
         this.sceneIndex = 3;
     }
    },
    LoadMap(data) {
      console.log(data);
      if (this.gameInstance != null) {
        this.gameInstance.SendMessage("SceneController", "ToggleMap", data);
      }
    }
  },
  mounted() {
    if (this.active) {
      this.gameInstance = UnityLoader.instantiate(
        "gameContainer",
        "../unity/Build/unity.json"
      );
    }
  }
};
</script>