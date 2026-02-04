<script setup>
import { ref } from 'vue'

import HeroSection from '@/components/HeroSection.vue'
import PlatformsSection from '@/components/PlatformsSection.vue'
import FeaturesSection from '@/components/FeaturesSection.vue'
import HowItWorksSection from '@/components/HowItWorksSection.vue'
import FinalCtaSection from '@/components/FinalCtaSection.vue'
import AboutSection from '@/components/AboutSection.vue'
import LeadModal from '@/components/LeadModal.vue'
import TeamCtaSection from '../components/TeamCtaSection.vue'
import FooterSection from '../components/FooterSection.vue'

const leadOpen = ref(false)
</script>

<template>
  <HeroSection @open-lead="leadOpen = true" />

  <PlatformsSection />
  <FeaturesSection />
  <TeamCtaSection @open-lead="leadOpen = true" />
  <HowItWorksSection @open-lead="leadOpen = true" />
  <FinalCtaSection @open-lead="leadOpen = true" />
  <AboutSection @open-lead="leadOpen = true" />
  <FooterSection />

  <LeadModal :open="leadOpen" @close="leadOpen = false" />
</template>
