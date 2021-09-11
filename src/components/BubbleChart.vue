<script>
import { Bubble } from "vue-chartjs";

export default {
  extends: Bubble,
  props: ["chartdata", "options"],
  mounted() {
    this.renderChart(this.chartdata, this.options);
  },
};
</script>