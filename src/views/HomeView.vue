<template>
  <div class="map-container">
    <div id="mapContainer" />
    <div class="locations">
      <div class="location-container" v-for="(location, ind) in locations" :key="ind">
        <div
          @click="flyToLocation(location)"
          class="location"
        >
          <div>
            <h2 class="text-[14px]">{{ location.name }}</h2>
            <span>{{ location.date }}</span>
          </div>
          <img :src="location.img" alt="img" class="location-img" />
        </div>
        <div class="location-content">
          <p>{{ location.details }}</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import mapboxgl from "mapbox-gl";
export default {
  name: "HomeView",
  data() {
    return {
      map: null,
      locations: [
        {
          name: "Brushy Mountain State Penitentiary",
          date: "2019-10-26",
          img: require("../assets/thumbnail.jpeg"),
          details:
            "Dakota meets the team in Tennessee to investigate the dark corridors of Brushy Mountain State Penitentiary. Chelsea has a shocking confrontation with one of its most infamous inmates, while Tanner is threatened by an entity known as The Creeper.",
          coordinates: {
            latitude: 36.1052,
            longitude: -84.4529,
          },
        },
        {
          name: "Old South Pittsburg Hospital",
          date: "2019-11-02",
          img: require("../assets/thumbnail.jpeg"),
          details:
            "Dakota meets the team in Tennessee to investigate the dark corridors of Brushy Mountain State Penitentiary. Chelsea has a shocking confrontation with one of its most infamous inmates, while Tanner is threatened by an entity known as The Creeper.",
          coordinates: {
            latitude: 35.0021,
            longitude: -85.7147,
          },
        },
      ],
      accessToken: `${process.env.VUE_APP_API_KEY}`,
      currentMarkers: [],
    };
  },
  mounted() {
    mapboxgl.accessToken = this.accessToken;
    this.map = new mapboxgl.Map({
      container: "mapContainer",
      style: "mapbox://styles/mapbox/streets-v11",
      center: [103.811279, 1.345399],
      zoom: 5,
    });
    this.locations.map((location) => {
      this.addMarker(location);
    });
    this.initializeCollapse();
  },
  methods: {
    addMarker(location) {
      const { longitude, latitude } = location.coordinates;
      const marker = new mapboxgl.Marker()
        .setLngLat([longitude, latitude])
        .addTo(this.map);
      this.currentMarkers.push(marker);
      this.map.setCenter([longitude, latitude]);
      this.addPopup(location, marker);
    },

    flyToLocation(location) {
      const { longitude, latitude } = location.coordinates;
      const target = {
        center: [longitude, latitude],
        zoom: 10,
      };
      this.map.flyTo({
        ...target, // Fly to the selected target
        duration: 3000, // Animate over 12 seconds
        essential: false, // This animation is considered essential with
      });
    },
    // removeOldMarkers(){
    //   if (this.currentMarkers!==null) {
    //     for (var i = this.currentMarkers.length - 1; i >= 0; i--) {
    //       this.currentMarkers[i].remove();
    //     }
    //   }
    // },
    // for marker popup
    addPopup(location, marker) {
      const markerDiv = marker.getElement();
      var popup = new mapboxgl.Popup({ offset: [0, -35] }).setHTML(
        `<div>
          <img src="${location.img}" alt="img" class="popup-img">
          <p class="p-2">${location.name}</p>
        </div>`
      );
      // hover event listener
      markerDiv.addEventListener("mouseenter", () => popup.addTo(this.map));
      // markerDiv.addEventListener('mouseleave', () => popup.remove());

      // add popup to marker
      marker.setPopup(popup);
      // add marker to map
      marker.addTo(this.map);
    },

    initializeCollapse() {
      var acc = document.getElementsByClassName("location");
      for (var i = 0; i < acc.length; i++) {
        acc[i].addEventListener("click", function () {
          this.classList.toggle("active");
          var panel = this.nextElementSibling;
          if (panel.style.maxHeight) {
            panel.style.maxHeight = null;
          } else {
            panel.style.maxHeight = panel.scrollHeight + "px";
          }
        });
      }
    },
  },
};
</script>

<style>
.map-container {
  display: grid;
  grid-template-columns: 75% 25%;
  height: calc(100vh - 60px);
}
.locations {
  height: 100%;
  overflow-y: auto;
  box-shadow: -10px 0px 10px 1px #aaaaaa;
}
.location-container {
  transition: 0.4s;
}
.location {
  border-bottom: 1px solid #ccc;
  padding: 10px;
  cursor: pointer;
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.location-content {
  background: #fafafa;
  max-height: 0;
  overflow: hidden;
  transition: max-height 0.2s ease-out;
}
.location-content p{
  font-size: 14px;
  margin: 12px;
}
.location-img {
  width: 70px;
  height: 70px;
  border-radius: 12px;
}
.popup-img {
  width: 100%;
  height: 100px;
  object-fit: cover;
  border-top-left-radius: 10px;
  border-top-right-radius: 10px;
}
.mapboxgl-popup-content {
  min-width: 250px;
  padding: 0px;
  box-shadow: 0 1px 2px rgb(60 64 67 / 30%), 0 2px 6px 2px rgb(60 64 67 / 15%);
  border-radius: 10px;
}
.mapboxgl-popup-close-button {
  display: none !important;
}
</style>