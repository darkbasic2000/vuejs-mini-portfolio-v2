<script setup>

  import { computed, ref } from "vue"
  
  const objective = ref('')
  const educations = ref([])
  const experiences = ref([])
  const skills = ref([])
  
  const experiencesOrderByYear = computed(() => {
    return experiences.value.sort(function(a, b) { return b.to - a.to })
  })
  
  const educationsOrderByYear = computed(() => {
    return educations.value.sort(function(a, b) { return b.completion - a.completion })
  })

  const itens = ref([
    { title: 'Bootstrap', icon: 'mdi-bootstrap' },
    { title: 'CSS', icon: 'mdi-language-css3' },
    { title: 'HTML', icon: 'mdi-language-html5'},
    { title: 'JavaScript', icon: 'mdi-language-javascript' },
    { title: 'PHP', icon: 'mdi-language-php' },
    { title: 'Laravel', icon: 'mdi-laravel' },
    { title: 'NodeJS', icon: 'mdi-nodejs' },    
    { title: 'VueJS', icon: 'mdi-vuejs' },
    { title: 'Vuetify', icon: 'mdi-vuetify' },
    { title: 'Unity', icon: 'mdi-unity' }
  ])

  const companyRules = [
    value => {
      if(value?.trim().length > 2) return true
      return 'Empresa deve conter pelo menos 3 caracteres'            
    }
  ]

  const occupationRules = [
    value => {
      if(value?.trim().length > 2) return true
      return 'Cargo deve ter pelo menos 3 caracteres'            
    }
  ]

  const fromRules = [
    value => {
      if(/^\d+$/.test(value) && value?.length === 4) return true
      return 'Início deve conter 4 dígitos'      
    }
  ]

  const toRules = [
    value => {
      if(/^\d+$/.test(value) && value?.length === 4) return true
      return 'Término deve conter 4 dígitos'
    }
  ]

  const titleRules = [
    value => {
      if(value?.trim().length > 2) return true
      return 'Título deve conter pelo menos 3 caracteres'            
    }
  ]

  const schoolRules = [
    value => {
      if(value?.trim().length > 2) return true
      return 'Escola deve conter pelo menos 3 caracteres'
    }
  ]

  const levelRules = [
    value => {
      if(value?.length > 0) return true
      return 'Selecione uma opção'            
    }
  ]

  const completionRules = [
    value => {
      if(/^\d+$/.test(value) && value?.length === 4) return true
      return 'Data de conclusão deve conter 4 dígitos'
    }
  ]
 
  async function addExperience(submitEventPromise) {
    
    const { valid } = await submitEventPromise    
    
    if(valid && experiences.value.length < 3) {
      const form = document.querySelector("#experienceForm")      
      const company = form.company.value
      const occupation = form.occupation.value
      const from = form.from.value
      const to = form.to.value
      const experience = { company, occupation, from, to }
      experiences.value.push(experience)
      form.reset() 
    }

  }

  async function addEducation(submitEventPromise) {
    
    const { valid } = await submitEventPromise    
    
    if(valid && educations.value.length < 3) {
      const form = document.querySelector("#educationForm")      
      const level = form.level.value      
      const title = form.title.value
      const school = form.school.value
      const completion = form.completion.value      
      const education = { level, title, school, completion }
      educations.value.push(education)
      form.reset()
    }

  }

  function addSkill(newSkill) {
    
    let exists = false
    skills.value.forEach(skill => {
        if(newSkill.title == skill.title) {
        exists = true        
      }
    })      
    if(!exists) {
      skills.value.push({ title: newSkill.title, icon: newSkill.icon })
      itens.value = itens.value.filter(item => item.title !== newSkill.title)      
    }

  }
 
</script>

<template>
  <div class="container d-flex">
    <div class="settings ps-2 pe-2">
      <v-sheet>
        <v-textarea label="Objetivo" v-model="objective" placeholder="Digite seu objetivo com poucas palavras" rows="3" maxlength="200" no-resize></v-textarea>
      </v-sheet>
      <v-sheet width="400" class="mt-1">
        <v-form fast-fail @submit.prevent="addEducation" id="educationForm">
          <div class="text-h6 text-center">Adicionar Formação</div>
          <v-select name="level" label="Selecionar" :rules="levelRules" :items="['Curso Técnico', 'Graduação', 'Especialização', 'Mestrado', 'Doutorado']" required>
            <template v-slot:counter></template>
          </v-select>
          <v-text-field name="title" label="Título" maxlength="30" :rules="titleRules" required></v-text-field>
          <v-text-field name="school" label="Escola" maxlength="30" :rules="schoolRules" required></v-text-field>
          <v-text-field name="completion" label="Conclusão (ano)" :rules="completionRules" maxlength="4"></v-text-field>          
          <v-btn type="submit" prepend-icon="mdi-book-plus" block class="mt-2">Adicionar</v-btn>
        </v-form>
      </v-sheet>
      <v-sheet width="400" class="mt-5">
        <v-form fast-fail @submit.prevent="addExperience" id="experienceForm">
          <div class="text-h6 text-center">Adicionar Experiência</div>
          <v-text-field name="company" label="Empresa" :rules="companyRules" maxlength="30"></v-text-field>
          <v-text-field name="occupation" label="Cargo" :rules="occupationRules" maxlength="30"></v-text-field>
          <v-text-field name="from" label="Início (ano)" :rules="fromRules" maxlength="4"></v-text-field>
          <v-text-field name="to" label="Término (ano)" :rules="toRules" maxlength="4"></v-text-field>
          <v-btn type="submit" prepend-icon="mdi-briefcase-plus" block class="mt-2">Adicionar</v-btn>
        </v-form>
      </v-sheet>      
    </div>
    <div class="portfolio ps-2 pe-2 d-flex flex-column">     
      <v-sheet>        
          <v-card>
            <v-img height="150" src="https://picsum.photos/150" cover>
              <div class="pa-5 font-weight-bold text-h6">{{ objective }}</div>
              <div class="d-flex">
                <v-sheet class="d-flex flex-column"></v-sheet>
                <v-sheet class="d-flex flex-column"></v-sheet>
              </div>
            </v-img>
            <div class="d-flex">
              <v-card-text>
                <div class="font-weight-bold ms-1 mb-1 text-center">
                  <v-icon icon="mdi-briefcase-variant"></v-icon>
                </div>
                <v-timeline density="compact" align="start">
                  <v-timeline-item v-for="(experience, index) in experiencesOrderByYear" :key="index" :dot-color="(index + 1) % 2 === 0 ? 'deep-purple-lighten-1' : 'green'" size="x-small">
                    <div class="mb-1">
                      <div class="font-weight-normal">
                        <strong>{{ experience.occupation }}</strong>
                      </div>
                      <div>{{ experience.company }} ({{ experience.from }} - {{ experience.to }})</div>
                    </div>
                  </v-timeline-item>
                </v-timeline>
              </v-card-text>
              <v-card-text>
                <div class="font-weight-bold ms-1 mb-1 text-center">
                  <v-icon icon="mdi-school"></v-icon>
                </div>
                <v-timeline density="compact" align="start">
                  <v-timeline-item v-for="(education, index) in educationsOrderByYear" :key="index" :dot-color="(index + 1) % 2 === 0 ? 'deep-purple-lighten-1' : 'green'" size="x-small">
                    <div class="mb-1">
                      <div class="font-weight-normal">
                        <strong>{{ education.level }}</strong> [ {{ education.school }} ]
                      </div>
                      <div>{{ education.title }} ({{ education.completion }})</div>
                    </div>
                  </v-timeline-item>
                </v-timeline>
              </v-card-text>
            </div>
            <v-sheet class="pa-5 d-flex flex-wrap">
              <div v-for="skill in skills" :key="skill.title">
                <v-chip class="ma-2" color="primary" label>
                  <v-icon start :icon="skill.icon"></v-icon>{{ skill.title }}
                </v-chip>
              </div>
            </v-sheet>
          </v-card>
      </v-sheet>
      <v-sheet class="mt-5">
        <v-select name="level" label="Selecionar habilidades" @update:modelValue="addSkill" return-object :items="itens" item-title="title" item-value="icon">
          <template v-slot:counter></template>
        </v-select>
      </v-sheet>
    </div>
  </div>
</template>

<style scoped>
.portfolio, .settings {
  width: 50%;
}
</style>
