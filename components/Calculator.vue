<template>
  <div>
    <div class="row align-items-center">
        <div class="col">
          <b-form-textarea
            id="originalcups"
            v-model="originalcups"
            placeholder="Copia y pega los cups sin la letra de validación (1 por línea)"
            rows="8"
          ></b-form-textarea>
        </div>
        <div class="col">
          <b-form-textarea
            id="convertedcups"
            v-model="convertedcups"
            placeholder="cups con la letra de validación"
            rows="8"
          ></b-form-textarea>
        </div>
      </div>
      <b-button @click="convertcups()">Convertir</b-button>
    </div>
</template>

<script>

function calculatecups(cups, validatedcups) {
  const cupspattern = /(?:[A-Z]{2})?(\d+)(?:[A-Z]{2})?/gm;
  const validated = cupspattern.exec(cups);

  if (!validated || validated.length < 1 || validated[1].length != 16){
    return cups + " not valid";
  }

  const cupsnumber = validated[1];

  const firstmodule = parseInt(cupsnumber) % 529;
  console.log(firstmodule);
  const finalmodule = Math.floor(firstmodule / 23);
  const finalrest = firstmodule % 23;

  const cupsletters = ['T','R','W','A','G','M','Y','F','P','D','X','B','N','J','Z','S','Q','V','H','L','C','K','E'];
  const cupscountry = cups.match(/([A-Z]{2})/gm);

  return (cupscountry?cupscountry[0]:'ES') + cupsnumber + cupsletters[finalmodule] + cupsletters[finalrest];
}

export default {
  data() {
    return {
      originalcups: '',
      convertedcups: ''
    }
  },
  methods: {
    convertcups(){
      this.convertedcups = '';

      const allcups = this.originalcups.split('\n');
      allcups.forEach(cups => {
        this.convertedcups += calculatecups(cups) + "\n";
      });
    }
  }
}
</script>
