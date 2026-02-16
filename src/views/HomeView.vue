<script setup>
import { ref } from 'vue'

import HeroSection from '@/components/HeroSection.vue'
import PlatformsSection from '@/components/PlatformsSection.vue'
import FeaturesSection from '@/components/FeaturesSection.vue'
import HowItWorksSection from '@/components/HowItWorksSection.vue'
import FinalCtaSection from '../components/FinalCtaSection.vue'
import AboutSection from '@/components/AboutSection.vue'
import LeadModal from '@/components/LeadModal.vue'
import TeamCtaSection from '../components/TeamCtaSection.vue'
import FooterSection from '../components/FooterSection.vue'

const leadOpen = ref(false)
const leadGoal = ref('submit_form_learn_more')

function openLead(goalId = 'submit_form_learn_more') {
  leadGoal.value = goalId
  leadOpen.value = true
}
</script>

<template>
  <HeroSection @open-lead="() => openLead('submit_form_learn_more')" />

  <PlatformsSection @open-lead="() => openLead('submit_form_from_tooltip')" />
  <FeaturesSection />
  <TeamCtaSection @open-lead="() => openLead('submit_form_free_consultation')" />
  <HowItWorksSection />
  <FinalCtaSection @open-lead="() => openLead('submit_form_want_see')" />
  <AboutSection />
  <FooterSection @open-lead="() => openLead('submit_form_free_consultation')" />

  <LeadModal :open="leadOpen" :goal-id="leadGoal" @close="leadOpen = false" />
</template>
