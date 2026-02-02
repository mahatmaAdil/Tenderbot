<script setup>
import { ref } from "vue";

import HeroSection from "../components/HeroSection.vue";
import PlatformsSection from "../components/PlatformsSection.vue";
import FeaturesSection from "../components/FeaturesSection.vue";
import HowItWorksSection from "../components/HowItWorksSection.vue";
import AboutSection from "../components/AboutSection.vue";
import FinalCtaSection from "../components/FinalCtaSection.vue";
import LeadModal from "../components/LeadModal.vue";

const leadOpen = ref(false);
const openLead = () => (leadOpen.value = true);
const closeLead = () => (leadOpen.value = false);
</script>

<template>
  <HeroSection @open-lead="openLead" />

  <PlatformsSection />
  <FeaturesSection />
  <HowItWorksSection @open-lead="openLead" />
  <AboutSection />
  <FinalCtaSection @open-lead="openLead" />

  <LeadModal :open="leadOpen" @close="closeLead" />
</template>
