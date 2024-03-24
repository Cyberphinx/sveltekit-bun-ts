<script lang="ts">
    import { onMount } from "svelte";

    onMount(async () => {
        const WebMap = (await import("@arcgis/core/WebMap")).default;
        const MapView = (await import("@arcgis/core/views/MapView")).default;
        const Bookmarks = (await import("@arcgis/core/widgets/Bookmarks"))
            .default;
        const Expand = (await import("@arcgis/core/widgets/Expand")).default;

        const webmap = new WebMap({
            portalItem: {
                id: "aa1d3f80270146208328cf66d022e09c",
            },
        });

        const view = new MapView({
            container: "viewDiv",
            map: webmap,
        });

        const bookmarks = new Bookmarks({
            view,
            // allows bookmarks to be added, edited, or deleted
            editingEnabled: true,
        });

        const bkExpand = new Expand({
            view,
            content: bookmarks,
            expanded: true,
        });

        // Add the widget to the top-right corner of the view
        view.ui.add(bkExpand, "top-right");

        // bonus - how many bookmarks in the webmap?
        view.when(() => {
            if (webmap.bookmarks && webmap.bookmarks.length) {
                console.log("Bookmarks: ", webmap.bookmarks.length);
            } else {
                console.log("No bookmarks in this webmap.");
            }
        });

        view.when(() => {
            console.debug("Map loaded");
        });
    });
</script>

<div id="viewDiv" />

<style>
    @import "https://js.arcgis.com/4.29/@arcgis/core/assets/esri/themes/light/main.css";
    #viewDiv {
        height: 500px;
    }
</style>
