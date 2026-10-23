# Consigli per affrontare un problema
 
<div class="flex flex-col gap-4 mt-5">
  <div class="box flex items-start gap-4">
    <span class="shrink-0 w-8 h-8 rounded-full bg-primary text-[#141418] font-bold flex items-center justify-center">1</span>
    <div>
      <strong>Individua subito il sottoproblema.</strong> Chiarisci che cosa rappresenta <code>dp[...]</code>, definisci anche il <strong>caso base</strong> e verifica che sia davvero immediato da calcolare.
    </div>
  </div>
  <div class="box flex items-start gap-4">
    <span class="shrink-0 w-8 h-8 rounded-full bg-primary text-[#141418] font-bold flex items-center justify-center">2</span>
    <div>
      <strong>Chiediti quali opzioni hai.</strong> Per il sottoproblema <em>i</em>-esimo: prendo o non prendo un elemento? Salto o mi muovo? Fissata una scelta, <strong>come uso le soluzioni ai problemi precedenti</strong> per calcolare quella attuale?
    </div>
  </div>
  <div class="box flex items-start gap-4">
    <span class="shrink-0 w-8 h-8 rounded-full bg-primary text-[#141418] font-bold flex items-center justify-center">3</span>
    <div>
      <strong>Concentrati sulla sovrapposizione.</strong> Dedica la maggior parte delle energie a capire <strong>dove</strong> sta e <strong>come</strong> sfruttarla: quali sottoproblemi ho già risolto e mi servono per calcolare il prossimo?
    </div>
  </div>
</div>

<!-- > **In sintesi** — se sai rispondere a queste tre domande, la ricorrenza è già scritta. -->
