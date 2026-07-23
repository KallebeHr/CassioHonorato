<template >
 <div class="t">
  <compteste />
  <servicos />
  <after />
  <clinica />
  <ComoChegar />
  <contatos />
  <navfooter/>
</div>
</template>

<script  setup>
import compteste from '@/components/compteste.vue';
import Servicos from '@/components/servicos.vue';
import After from '@/components/after.vue';
import ComoChegar from '@/components/comoChegar.vue';
import Contatos from '@/components/contatos.vue';
import Navfooter from '@/components/navfooter.vue';

</script>
<style>
.t{
  background: #f9f8f3;

}
.dust {
  position: fixed;
  width: 5px;
  height: 5px;
  border-radius: 50%;
  pointer-events: none;
  z-index: 50;
  background: radial-gradient(circle, #e8c468, transparent 70%);
  animation: dust-fade 0.9s ease-out forwards;
}
@keyframes dust-fade {
  0% { opacity: 0.9; transform: translateY(0) scale(1); }
  100% { opacity: 0; transform: translateY(-26px) scale(0.3); }
}
 
.burst-particle {
  position: fixed;
  width: 6px;
  height: 6px;
  border-radius: 50%;
  background: #c9a227;
  pointer-events: none;
  z-index: 60;
  animation: burst-fly 0.7s ease-out forwards;
}
@keyframes burst-fly {
  to { transform: translate(var(--tx), var(--ty)) scale(0); opacity: 0; }
}
</style>
 
