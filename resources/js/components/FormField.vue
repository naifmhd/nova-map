<template>
  <default-field :field="field">
    <template slot="field">
      <div>{{ value }}</div>

      <div class="rounded flex flex-col w-full" :style="'height: 300px'">
        <map-detail
          :type="field.spatialType"
          :geojson="field.value"
          :edit="true"
        ></map-detail>
      </div>

      <input
        :id="field.name"
        type="text"
        class="w-full form-control form-input form-input-bordered"
        :class="errorClasses"
        :placeholder="field.name"
        v-model="value"
      />

      <p v-if="hasError" class="my-2 text-danger">
        {{ firstError }}
      </p>
    </template>
  </default-field>
</template>

<script>
import { FormField, HandlesValidationErrors } from "laravel-nova";
import { Icon } from "leaflet";

delete Icon.Default.prototype._getIconUrl;
Icon.Default.mergeOptions({
  iconRetinaUrl: require("leaflet/dist/images/marker-icon-2x.png"),
  iconUrl: require("leaflet/dist/images/marker-icon.png"),
  shadowUrl: require("leaflet/dist/images/marker-shadow.png"),
});
export default {
  mixins: [FormField, HandlesValidationErrors],

  props: ["resourceName", "resourceId", "field"],

  data() {
    return {
      bounds: null,
    };
  },
  mounted() {},

  methods: {
    setInitialValue() {
      this.value = this.field.value || "";
    },

    fill(formData) {
      formData.append(this.field.attribute, JSON.stringify(this.value) || "");
    },

    handleChange(value) {
      this.value = value;
    },
  },
};
</script>
