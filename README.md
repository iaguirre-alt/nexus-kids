# nexus-kids
Plataforma NEXUS KIDS
import React, { useState } from "react";
import { Brain, Heart, Users, Zap, Eye, Lightbulb, BarChart3, Bell, Target, TrendingUp, BookOpen, AlertTriangle, FileText, Settings, Star, Shield, GraduationCap, Clipboard, MessageSquare, Home, ChevronLeft, Edit3, LogOut, School, UserPlus, Clock, Play, Award, Send, Calendar, Activity, PenTool, ChevronRight, Sparkles, ArrowUp, ArrowDown, CheckCircle, XCircle, Info, PieChart, Layers, Plus, Filter, Download, Printer, BarChart2, List, Menu, X } from "lucide-react";

const F="'Plus Jakarta Sans',system-ui,sans-serif",FD="'DM Serif Display',Georgia,serif";
const P="#0D9488",PD="#0F766E",PL="#F0FDFA",BG="#F8FAFB",TX="#0F172A",TM="#64748B",TL="#94A3B8",BD="#E2E8F0",BL="#F1F5F9",DARK="#0C3C36";
const inp={fontFamily:F,fontSize:13,padding:"10px 14px",borderRadius:10,border:"1px solid "+BD,outline:"none",width:"100%",boxSizing:"border-box",background:"#fff",color:TX};
const cd=x=>({background:"#fff",borderRadius:14,border:"1px solid "+BD,boxShadow:"0 1px 3px rgba(0,0,0,.05)",...(x||{})});
const bp=s=>({fontFamily:F,fontSize:s?11:13,fontWeight:600,padding:s?"7px 14px":"11px 20px",borderRadius:10,border:"none",background:"linear-gradient(135deg,"+P+","+PD+")",color:"#fff",cursor:"pointer"});
const bo=(on,s)=>({fontFamily:F,fontSize:s?11:13,fontWeight:on?600:400,padding:s?"6px 12px":"9px 16px",borderRadius:10,border:on?"1.5px solid "+P:"1.5px solid "+BD,background:on?PL:"#fff",color:on?PD:TM,cursor:"pointer"});
const DOM=[{k:"cog",l:"Cognitivo",I:Brain,c:"#0EA5E9",sub:["Lenguaje expresivo","Lenguaje receptivo","Razonamiento lógico","Memoria de trabajo","Atención sostenida"]},{k:"emo",l:"Emocional",I:Heart,c:"#EC4899",sub:["Regulación emocional","Autoconciencia","Autoestima","Tol. frustración","Expresión emocional"]},{k:"soc",l:"Social",I:Users,c:"#3B82F6",sub:["Cooperación","Empatía","Comunicación social","Resolución conflictos","Vínculo con pares"]},{k:"mot",l:"Motor",I:Zap,c:"#F59E0B",sub:["Motricidad fina","Motricidad gruesa","Coordinación","Lateralidad","Equilibrio"]},{k:"sen",l:"Sensorial",I:Eye,c:"#8B5CF6",sub:["Proc. visual","Proc. auditivo","Integración sensorial","Propiocepción","Tolerancia sensorial"]},{k:"mtv",l:"Motivacional",I:Lightbulb,c:"#10B981",sub:["Curiosidad","Persistencia","Iniciativa","Foco atencional","Autonomía"]}];
const ITEMS={cog:DOM[0].sub,emo:DOM[1].sub,soc:DOM[2].sub,mot:DOM[3].sub,sen:DOM[4].sub,mtv:DOM[5].sub};
const gs=sc=>sc?Math.round(DOM.reduce((s,d)=>s+(sc[d.k]||0),0)/6):0;
const uid=()=>"id"+Math.random().toString(36).slice(2,9);
const td=()=>new Date().toISOString().slice(0,10);
const fmt=d=>{try{return new Date(d+"T00:00:00").toLocaleDateString("es-ES",{day:"numeric",month:"short"})}catch{return d}};
const ageFn=dob=>{const n=new Date(),b=new Date(dob||"2021-01-01");let y=n.getFullYear()-b.getFullYear(),m=n.getMonth()-b.getMonth();if(m<0){y--;m+=12;}return y+"a "+m+"m";};

/* Percentile tables by age and domain (Bayley-III, ASQ-3, TPBA-2, Merrill-Palmer-R) */
const PCTL={3:{P5:28,P10:35,P25:48,P50:62,P75:76,P90:85,P95:92},4:{P5:33,P10:40,P25:52,P50:65,P75:79,P90:88,P95:94},5:{P5:38,P10:45,P25:55,P50:68,P75:82,P90:90,P95:95},6:{P5:42,P10:48,P25:58,P50:72,P75:84,P90:92,P95:96}};
const PCTL_DOM={cog:{3:{P10:35,P25:48,P50:62,P90:85},4:{P10:40,P25:52,P50:65,P90:88},5:{P10:45,P25:55,P50:68,P90:90},6:{P10:48,P25:58,P50:72,P90:92}},emo:{3:{P10:32,P25:45,P50:60,P90:82},4:{P10:38,P25:50,P50:63,P90:86},5:{P10:42,P25:53,P50:66,P90:88},6:{P10:45,P25:56,P50:70,P90:90}},soc:{3:{P10:30,P25:44,P50:58,P90:80},4:{P10:36,P25:48,P50:62,P90:84},5:{P10:40,P25:52,P50:65,P90:87},6:{P10:44,P25:55,P50:70,P90:90}},mot:{3:{P10:38,P25:50,P50:65,P90:88},4:{P10:42,P25:55,P50:68,P90:90},5:{P10:48,P25:58,P50:72,P90:92},6:{P10:50,P25:60,P50:75,P90:94}},sen:{3:{P10:33,P25:46,P50:60,P90:83},4:{P10:38,P25:50,P50:64,P90:86},5:{P10:43,P25:54,P50:67,P90:89},6:{P10:46,P25:57,P50:71,P90:91}},mtv:{3:{P10:34,P25:47,P50:61,P90:84},4:{P10:39,P25:51,P50:64,P90:87},5:{P10:44,P25:55,P50:68,P90:90},6:{P10:47,P25:57,P50:72,P90:92}}};
const getAge=dob=>{const n=new Date(),b=new Date(dob||"2021-01-01");return n.getFullYear()-b.getFullYear()+(n.getMonth()-b.getMonth())/12;};
const getAgeY=dob=>Math.min(6,Math.max(3,Math.round(getAge(dob))));
const getPctl=dob=>{const a=getAgeY(dob);return PCTL[a]||PCTL[4];};
const getDomPctl=(dom,dob)=>{const a=getAgeY(dob);return (PCTL_DOM[dom]||{})[a]||{P10:40,P25:52,P50:65,P90:88};};
const percLb=v=>v>=75?"Excelente":v>=50?"Adecuado":v>=30?"En desarrollo":"Necesita apoyo";
const percCo=v=>v>=75?"#059669":v>=50?P:v>=30?"#D97706":"#DC2626";
const percBg=v=>v>=75?"#ECFDF5":v>=50?PL:v>=30?"#FFFBEB":"#FEF2F2";
const sevLb=(v,pctl)=>v<pctl.P10?"danger":v<pctl.P25?"warning":v>pctl.P90?"excellent":"normal";
const sevCo=s=>s==="danger"?"#DC2626":s==="warning"?"#D97706":s==="excellent"?"#059669":P;
const sevBg=s=>s==="danger"?"#FEF2F2":s==="warning"?"#FFFBEB":s==="excellent"?"#ECFDF5":PL;
const pctlLabel=(v,pctl)=>v<pctl.P5?"<P5":v<pctl.P10?"P5-P10":v<pctl.P25?"P10-P25":v<pctl.P50?"P25-P50":v<pctl.P75?"P50-P75":v<pctl.P90?"P75-P90":"P90+";

/* Activities catalog - 24 activities across 6 domains */
const ACT_SUBS={cog:["Memoria","Atención","Lenguaje","Razonamiento","Funciones ejecutivas"],emo:["Regulación","Autoconciencia","Expresión","Tol. frustración","Autoestima"],soc:["Cooperación","Empatía","Comunicación","Resolución conflictos","Normas sociales"],mot:["M. fina","M. gruesa","Equilibrio","Coordinación","Lateralidad"],sen:["Táctil","Auditivo","Visual","Propioceptivo","Multisensorial"],mtv:["Iniciativa","Persistencia","Curiosidad","Autonomía","Creatividad"]};
const ageYears=dob=>{const n=new Date(),b=new Date(dob||"2021-01-01");let y=n.getFullYear()-b.getFullYear();if(n.getMonth()<b.getMonth()||(n.getMonth()===b.getMonth()&&n.getDate()<b.getDate()))y--;return y;};
const PMI_RECS={
cog:{
"2-3":["Ampliar vocabulario a 50+ palabras","Comprender instrucciones de un paso","Señalar objetos familiares cuando se nombran","Hacer torres de 6+ bloques","Emparejar colores básicos"],
"3-4":["Contar hasta 10 de forma consistente","Reconocer 4+ colores por nombre","Seguir instrucciones de 2 pasos","Mantener atención en una actividad 5+ min","Usar frases de 3-4 palabras"],
"4-5":["Contar hasta 20 y reconocer números 1-10","Comprender conceptos de antes/después","Clasificar objetos por 2 criterios","Relatar experiencias con secuencia temporal","Mantener atención sostenida 10+ min"],
"5-6":["Reconocer letras del abecedario","Resolver problemas sencillos paso a paso","Comprender instrucciones de 3+ pasos","Narrar historias con inicio, nudo y desenlace","Mantener atención en tareas de 15+ min"]
},emo:{
"2-3":["Identificar emociones básicas (alegre/triste)","Aceptar separación del cuidador sin llanto prolongado","Comenzar a usar palabras para expresar necesidades","Tolerar pequeñas frustraciones sin rabieta intensa"],
"3-4":["Nombrar 4 emociones básicas","Empezar a regular el enfado con ayuda","Expresar verbalmente lo que quiere","Tolerar esperar su turno 2-3 min","Mostrar orgullo al completar tareas"],
"4-5":["Identificar emociones en otros niños","Usar estrategias de calma con guía","Expresar emociones con palabras en vez de conducta","Tolerar cambios de rutina sin gran angustia","Gestionar la frustración ante el error"],
"5-6":["Explicar por qué se siente de cierta forma","Usar estrategias de autorregulación autónomamente","Reconocer cómo sus acciones afectan a otros","Aceptar perder en juegos sin desregularse","Mostrar empatía ante el malestar ajeno"]
},soc:{
"2-3":["Jugar al lado de otros niños (juego paralelo)","Compartir objetos con ayuda del adulto","Responder cuando le llaman por su nombre","Imitar acciones de otros niños"],
"3-4":["Participar en juego cooperativo sencillo","Esperar turno con recordatorio","Compartir materiales con compañeros","Pedir ayuda verbalmente","Saludar y despedirse"],
"4-5":["Jugar cooperativamente con reglas simples","Resolver conflictos con palabras antes que con el cuerpo","Mostrar empatía cuando un compañero llora","Respetar normas del aula con recordatorios mínimos","Participar en conversaciones grupales"],
"5-6":["Negociar roles en el juego","Resolver conflictos de forma autónoma","Incluir a compañeros nuevos o aislados","Respetar normas sociales sin supervisión constante","Trabajar en equipo con un objetivo común"]
},mot:{
"2-3":["Subir y bajar escaleras con apoyo","Pasar páginas de un libro una a una","Hacer garabatos circulares","Lanzar una pelota hacia delante","Apilar 6+ bloques"],
"3-4":["Correr cambiando de dirección","Recortar en línea recta con tijeras","Copiar un círculo y una cruz","Saltar con los dos pies juntos","Abrochar botones grandes"],
"4-5":["Cortar siguiendo una línea curva","Copiar letras y números sencillos","Saltar a la pata coja 3+ veces","Lanzar y atrapar una pelota grande","Abrocharse la cremallera"],
"5-6":["Escribir su nombre de forma legible","Recortar figuras complejas","Saltar a la comba","Atarse los cordones con ayuda","Mantener equilibrio sobre un pie 10+ seg"]
},sen:{
"2-3":["Tolerar diferentes texturas en las manos","Responder al escuchar su nombre","Explorar materiales sensoriales sin rechazo","Identificar sonidos familiares del entorno"],
"3-4":["Tolerar ruidos moderados sin taparse los oídos","Manipular plastilina y arena sin rechazo","Identificar objetos por el tacto","Discriminar sonidos fuertes vs suaves","Seguir objetos con la mirada sin mover la cabeza"],
"4-5":["Discriminar sonidos similares","Completar puzzles de 12+ piezas","Tolerar actividades con texturas variadas","Copiar patrones visuales sencillos","Identificar 4+ sabores y olores básicos"],
"5-6":["Encontrar diferencias en imágenes detalladas","Seguir instrucciones auditivas complejas","Tolerar estímulos sensoriales intensos (ruido comedor)","Coordinar vista y mano en tareas de precisión","Reproducir secuencias rítmicas de 4+ golpes"]
},mtv:{
"2-3":["Mostrar interés por explorar objetos nuevos","Intentar hacer cosas por sí mismo","Señalar cosas que le interesan","Persistir en una actividad 2-3 min"],
"3-4":["Elegir actividades por iniciativa propia","Completar tareas sencillas sin abandonar","Hacer preguntas sobre el entorno","Mostrar satisfacción al terminar una tarea","Intentar resolver antes de pedir ayuda"],
"4-5":["Mantener interés en un proyecto varios días","Aceptar desafíos nuevos sin negarse","Proponer ideas propias en actividades","Persistir ante dificultades moderadas","Mostrar curiosidad por cómo funcionan las cosas"],
"5-6":["Planificar una tarea sencilla antes de hacerla","Establecer y trabajar hacia metas personales","Proponer actividades nuevas al grupo","Persistir ante la frustración y buscar alternativas","Autoevaluar su propio trabajo con criterio"]
}};
const MILE_RECS={
"2-3":{cog:["Dijo su primera frase de 2 palabras","Reconoce 3+ colores","Cuenta hasta 5","Entiende conceptos grande/pequeño","Nombra objetos familiares en imágenes"],emo:["Nombró una emoción por primera vez","Se despide del cuidador sin llanto","Muestra afecto espontáneo a compañeros","Acepta una norma sin rabieta","Se calma solo/a tras un enfado breve"],soc:["Jugó con otro niño por primera vez","Compartió un juguete sin que se lo pidieran","Dijo 'por favor' o 'gracias' espontáneamente","Participó en una canción grupal","Imitó acciones de un compañero"],mot:["Subió escaleras alternando pies","Hizo su primer dibujo reconocible","Consiguió comer solo/a con cuchara","Saltó con los dos pies juntos","Encajó piezas en un puzzle de 4+"],sen:["Toleró pintar con los dedos","Identificó un sonido del entorno","Exploró arena o agua sin rechazo","Reaccionó adecuadamente a un olor nuevo"],mtv:["Eligió una actividad por sí mismo/a","Completó un puzzle sin ayuda","Pidió repetir una actividad que le gustó","Mostró un objeto encontrado con entusiasmo"]},
"3-4":{cog:["Cuenta hasta 10 solo/a","Reconoce su nombre escrito","Sigue instrucciones de 2 pasos","Clasifica por color o forma","Recuerda una canción entera","Entiende ayer/hoy/mañana"],emo:["Nombra 4 emociones en sí mismo","Usa palabras para expresar enfado","Acepta perder en un juego","Se calma usando una estrategia aprendida","Muestra orgullo al completar algo difícil"],soc:["Juega cooperativamente con 2+ niños","Espera su turno sin recordatorio","Resuelve un conflicto con palabras","Consuela a un compañero que llora","Respeta una norma nueva del aula"],mot:["Recorta en línea recta","Copia un círculo bien","Salta a la pata coja","Sube a un tobogán solo/a","Se viste sin ayuda (excepto botones complejos)"],sen:["Discrimina sonidos de instrumentos","Completa puzzle de 12 piezas","Tolera el ruido del comedor sin taparse","Identifica texturas con los ojos cerrados"],mtv:["Termina una actividad de 10+ min","Elige un tema para investigar","Intenta algo difícil sin rendirse","Hace preguntas sobre el mundo","Propone un juego nuevo"]},
"4-5":{cog:["Cuenta hasta 20","Reconoce números del 1-10","Escribe las primeras letras","Comprende conceptos temporales","Relata una experiencia en orden","Resuelve adivinanzas sencillas"],emo:["Explica por qué se siente así","Usa respiración para calmarse","Reconoce emociones en los demás","Acepta cambios de plan sin desregularse","Muestra empatía espontáneamente"],soc:["Negocia roles en el juego","Incluye a un compañero aislado","Lidera una actividad grupal","Respeta turnos sin supervisión","Media en un conflicto entre compañeros"],mot:["Escribe su nombre","Recorta figuras curvas","Salta a la comba","Se ata velcro/hebilla solo","Atrapa pelota con dos manos"],sen:["Encuentra 8+ diferencias en imágenes","Reproduce secuencia de 4 sonidos","Tolera estímulos táctiles variados","Completa laberintos visuales"],mtv:["Trabaja en un proyecto durante una semana","Propone mejoras a una actividad","Se autoevalúa tras una tarea","Persiste ante 3+ intentos fallidos","Explica qué quiere aprender"]},
"5-6":{cog:["Lee palabras sencillas","Hace sumas simples con objetos","Escribe frases cortas","Comprende causa-efecto complejos","Memoriza secuencias de 5+ elementos","Categoriza objetos por 3 criterios"],emo:["Gestiona frustración de forma autónoma","Identifica emociones complejas (vergüenza, celos)","Reflexiona sobre su comportamiento","Acepta críticas constructivas","Muestra resiliencia ante el fracaso"],soc:["Trabaja en equipo hacia un objetivo","Resuelve conflictos sin intervención adulta","Demuestra liderazgo positivo","Defiende a un compañero","Crea reglas de juego justas"],mot:["Escribe de forma legible en pauta","Ata cordones de zapatos","Recorta figuras complejas con detalle","Tiene dominancia lateral definida","Coordina ambas manos en tareas"],sen:["Discrimina sonidos similares con precisión","Integra múltiples sentidos simultáneamente","Localiza sonidos en el espacio","Mantiene atención visual en tareas de detalle"],mtv:["Planifica tareas antes de empezar","Establece metas personales semanales","Muestra pasión por un tema concreto","Evalúa su trabajo con criterio objetivo","Busca mejorar de forma autónoma"]}
};
const getAgeRange=(dob)=>{const y=ageYears(dob);if(y<=3)return"2-3";if(y<=4)return"3-4";if(y<=5)return"4-5";return"5-6";};
const ACTS=[
// ═══ COGNITIVO ═══
{dom:"cog",t:"Memory con tarjetas",desc:"Juego de parejas con 8-12 tarjetas. Refuerza memoria de trabajo y atención visual.",dur:"10 min",age:"3-5",dif:"Fácil",obj:"Memoria de trabajo",mats:"Tarjetas con imágenes pareadas",lvl:1,sub:"Memoria",tags:["atención","visual"]},
{dom:"cog",t:"Memory avanzado",desc:"Memory con 16-20 tarjetas incluyendo asociaciones temáticas (animal-habitat). Mayor demanda cognitiva.",dur:"15 min",age:"4-6",dif:"Media",obj:"Memoria avanzada",mats:"Tarjetas temáticas pareadas",lvl:2,sub:"Memoria",tags:["atención","categorización"]},
{dom:"cog",t:"Memory secuencial",desc:"Recordar secuencias de 4-6 elementos mostrados brevemente. Memoria de trabajo compleja.",dur:"15 min",age:"5-6",dif:"Alta",obj:"Memoria secuencial",mats:"Tarjetas de secuencias, temporizador",lvl:3,sub:"Memoria",tags:["concentración","secuenciación"]},
{dom:"cog",t:"Secuencias lógicas",desc:"Ordenar patrones de colores, formas o tamaños crecientes.",dur:"15 min",age:"4-6",dif:"Media",obj:"Razonamiento lógico",mats:"Fichas de colores y formas",lvl:1,sub:"Razonamiento",tags:["lógica","patrones"]},
{dom:"cog",t:"Patrones complejos",desc:"Completar series de 3 variables: color+forma+tamaño. Razonamiento abstracto inicial.",dur:"15 min",age:"5-6",dif:"Alta",obj:"Razonamiento abstracto",mats:"Fichas multivariable",lvl:2,sub:"Razonamiento",tags:["abstracción","lógica"]},
{dom:"cog",t:"Matrices lógicas",desc:"Encontrar la pieza que falta en una cuadrícula 2x2 o 3x3 siguiendo reglas lógicas.",dur:"20 min",age:"5-6",dif:"Alta",obj:"Razonamiento matricial",mats:"Fichas de matrices, plantillas",lvl:3,sub:"Razonamiento",tags:["abstracción","resolución"]},
{dom:"cog",t:"Cuentacuentos con preguntas",desc:"Leer un cuento corto y hacer 5 preguntas de comprensión. Estimula lenguaje receptivo.",dur:"15 min",age:"3-6",dif:"Fácil",obj:"Lenguaje receptivo",mats:"Cuento adaptado a edad",lvl:1,sub:"Lenguaje",tags:["comprensión","vocabulario"]},
{dom:"cog",t:"Inventar finales",desc:"Tras leer un cuento, el niño inventa un final alternativo. Lenguaje expresivo y creatividad.",dur:"15 min",age:"4-6",dif:"Media",obj:"Lenguaje expresivo",mats:"Cuentos con finales abiertos",lvl:2,sub:"Lenguaje",tags:["creatividad","narrativa"]},
{dom:"cog",t:"Narrador por turnos",desc:"Cada niño añade una frase a una historia colectiva. Memoria, coherencia y vocabulario.",dur:"20 min",age:"5-6",dif:"Alta",obj:"Narración compleja",mats:"Dados de historias o imágenes secuenciales",lvl:3,sub:"Lenguaje",tags:["narrativa","cooperación","memoria"]},
{dom:"cog",t:"Dictado de colores",desc:"Nombrar colores de objetos en secuencia. Ejercita atención sostenida y vocabulario.",dur:"10 min",age:"4-6",dif:"Media",obj:"Atención sostenida",mats:"Objetos variados de colores",lvl:1,sub:"Atención",tags:["vocabulario","concentración"]},
{dom:"cog",t:"Escucha activa",desc:"Seguir instrucciones verbales de 3-4 pasos en orden. Atención auditiva y memoria operativa.",dur:"10 min",age:"4-6",dif:"Media",obj:"Atención auditiva",mats:"Lista de instrucciones graduadas",lvl:2,sub:"Atención",tags:["instrucciones","secuenciación"]},
{dom:"cog",t:"Doble tarea",desc:"Realizar dos tareas simultáneas sencillas: colorear mientras escucha instrucciones.",dur:"15 min",age:"5-6",dif:"Alta",obj:"Atención dividida",mats:"Fichas para colorear, audio de instrucciones",lvl:3,sub:"Atención",tags:["multitarea","concentración"]},
{dom:"cog",t:"Clasificador de objetos",desc:"Agrupar objetos por categorías (animales, alimentos, transportes). Categorización básica.",dur:"10 min",age:"3-5",dif:"Fácil",obj:"Categorización",mats:"Objetos o imágenes variadas",lvl:1,sub:"Funciones ejecutivas",tags:["clasificación","vocabulario"]},
{dom:"cog",t:"Planificador de pasos",desc:"El niño explica cómo haría una tarea cotidiana paso a paso. Planificación secuencial.",dur:"10 min",age:"4-6",dif:"Media",obj:"Planificación",mats:"Imágenes de tareas cotidianas",lvl:2,sub:"Funciones ejecutivas",tags:["organización","lenguaje"]},
// ═══ EMOCIONAL ═══
{dom:"emo",t:"El semáforo emocional",desc:"Rojo=paro, Amarillo=pienso, Verde=actúo. Técnica de regulación emocional visual.",dur:"10 min",age:"3-6",dif:"Fácil",obj:"Regulación emocional",mats:"Tarjeta de semáforo plastificada",lvl:1,sub:"Regulación",tags:["autocontrol","visual"]},
{dom:"emo",t:"Respiración de la estrella",desc:"Trazar una estrella con el dedo mientras respira: inhala en una punta, exhala en la siguiente.",dur:"5 min",age:"3-6",dif:"Fácil",obj:"Calma corporal",mats:"Plantilla de estrella grande",lvl:1,sub:"Regulación",tags:["mindfulness","corporal"]},
{dom:"emo",t:"Termómetro emocional",desc:"Situar la intensidad de la emoción en un termómetro del 1-5. Graduar la respuesta.",dur:"10 min",age:"4-6",dif:"Media",obj:"Graduación emocional",mats:"Termómetro visual plastificado",lvl:2,sub:"Regulación",tags:["autoconciencia","intensidad"]},
{dom:"emo",t:"Mi plan de calma",desc:"Cada niño crea su plan personal con 3 estrategias para calmarse. Autorregulación avanzada.",dur:"15 min",age:"5-6",dif:"Alta",obj:"Autorregulación",mats:"Plantilla de plan, pegatinas de estrategias",lvl:3,sub:"Regulación",tags:["autonomía","planificación"]},
{dom:"emo",t:"Diario de emociones",desc:"Dibujar la emoción del día y compartirla con el grupo. Fomenta autoconciencia.",dur:"10 min",age:"4-6",dif:"Fácil",obj:"Autoconciencia",mats:"Folios y pinturas",lvl:1,sub:"Autoconciencia",tags:["expresión","dibujo"]},
{dom:"emo",t:"Rueda de emociones",desc:"Identificar emociones complejas más allá de las 4 básicas: vergüenza, orgullo, celos, sorpresa.",dur:"15 min",age:"5-6",dif:"Media",obj:"Vocabulario emocional",mats:"Rueda de emociones ilustrada",lvl:2,sub:"Autoconciencia",tags:["vocabulario","empatía"]},
{dom:"emo",t:"El frasco de la calma",desc:"Crear un frasco con purpurina y agua. Observar cómo se asienta para calmarse.",dur:"15 min",age:"3-5",dif:"Fácil",obj:"Tol. frustración",mats:"Bote transparente, purpurina, agua",lvl:1,sub:"Tol. frustración",tags:["sensorial","calma"]},
{dom:"emo",t:"Torre imposible",desc:"Construir una torre muy alta que se cae a propósito. Trabajar reacción ante la frustración.",dur:"10 min",age:"4-6",dif:"Media",obj:"Tolerancia al error",mats:"Bloques apilables",lvl:2,sub:"Tol. frustración",tags:["persistencia","autocontrol"]},
{dom:"emo",t:"Retos graduados",desc:"Afrontar 3 retos de dificultad creciente aceptando que puede fallar. Resiliencia.",dur:"15 min",age:"5-6",dif:"Alta",obj:"Resiliencia",mats:"Puzzles de 3 niveles de dificultad",lvl:3,sub:"Tol. frustración",tags:["persistencia","autoestima"]},
{dom:"emo",t:"Teatro de emociones",desc:"Representar emociones con el cuerpo. Los demás adivinan. Expresión emocional.",dur:"15 min",age:"3-6",dif:"Media",obj:"Expresión emocional",mats:"Tarjetas con emociones",lvl:1,sub:"Expresión",tags:["corporal","comunicación"]},
{dom:"emo",t:"Carta a mi emoción",desc:"Escribir/dibujar una carta a la emoción que siente. Externalización terapéutica.",dur:"15 min",age:"5-6",dif:"Media",obj:"Externalización",mats:"Folios, pinturas, sobre",lvl:2,sub:"Expresión",tags:["escritura","creatividad"]},
{dom:"emo",t:"Mi superpoder emocional",desc:"Cada niño identifica su fortaleza emocional y la comparte. Refuerzo de autoestima.",dur:"10 min",age:"4-6",dif:"Fácil",obj:"Autoestima",mats:"Tarjetas de fortalezas ilustradas",lvl:1,sub:"Autoestima",tags:["positivo","social"]},
// ═══ SOCIAL ═══
{dom:"soc",t:"Construcción cooperativa",desc:"Construir una torre entre 3-4 niños con turnos y negociación.",dur:"20 min",age:"4-6",dif:"Media",obj:"Cooperación",mats:"Bloques de construcción",lvl:1,sub:"Cooperación",tags:["turnos","motricidad"]},
{dom:"soc",t:"Proyecto en equipo",desc:"Crear un mural o maqueta grupal donde cada niño aporta su pieza. Roles definidos.",dur:"30 min",age:"5-6",dif:"Alta",obj:"Trabajo en equipo",mats:"Cartulinas, pegamento, materiales variados",lvl:2,sub:"Cooperación",tags:["planificación","creatividad"]},
{dom:"soc",t:"Cadena de favores",desc:"Cada niño hace un favor a otro y lo registra. Reflexión grupal al final.",dur:"Todo el día",age:"4-6",dif:"Media",obj:"Prosocialidad",mats:"Tablero de favores visual",lvl:3,sub:"Cooperación",tags:["empatía","autonomía"]},
{dom:"soc",t:"Teatro de roles",desc:"Representar situaciones sociales: compartir, pedir ayuda, disculparse.",dur:"15 min",age:"3-6",dif:"Fácil",obj:"Comunicación social",mats:"Disfraces y accesorios",lvl:1,sub:"Comunicación",tags:["lenguaje","expresión"]},
{dom:"soc",t:"Reportero del aula",desc:"Un niño entrevista a compañeros con preguntas preparadas. Escucha activa social.",dur:"15 min",age:"5-6",dif:"Media",obj:"Escucha activa",mats:"Micrófono de juguete, preguntas",lvl:2,sub:"Comunicación",tags:["lenguaje","atención"]},
{dom:"soc",t:"Debate en asamblea",desc:"Los niños debaten un tema sencillo respetando turnos de palabra. Argumentación básica.",dur:"20 min",age:"5-6",dif:"Alta",obj:"Argumentación",mats:"Tarjeta de turnos, tema propuesto",lvl:3,sub:"Comunicación",tags:["razonamiento","respeto"]},
{dom:"soc",t:"El juego del espejo",desc:"Imitar gestos y expresiones del compañero. Desarrolla empatía.",dur:"10 min",age:"3-5",dif:"Fácil",obj:"Empatía",mats:"Ninguno",lvl:1,sub:"Empatía",tags:["corporal","atención"]},
{dom:"soc",t:"¿Cómo se siente?",desc:"Observar fotos de niños y adivinar sus emociones. Teoría de la mente básica.",dur:"10 min",age:"4-6",dif:"Media",obj:"Reconocimiento emocional",mats:"Fotos de expresiones faciales",lvl:2,sub:"Empatía",tags:["emocional","visual"]},
{dom:"soc",t:"Consejero amigo",desc:"Un niño escucha el problema de otro y propone soluciones empáticas.",dur:"10 min",age:"5-6",dif:"Alta",obj:"Empatía avanzada",mats:"Tarjetas de situaciones",lvl:3,sub:"Empatía",tags:["resolución","emocional"]},
{dom:"soc",t:"Mediador de conflictos",desc:"Un niño ayuda a otros dos a resolver un desacuerdo con palabras.",dur:"15 min",age:"5-6",dif:"Alta",obj:"Resolución conflictos",mats:"Guion de mediación visual",lvl:2,sub:"Resolución conflictos",tags:["comunicación","autocontrol"]},
{dom:"soc",t:"Rueda de soluciones",desc:"Ante un conflicto, el grupo propone soluciones y elige la mejor por votación.",dur:"15 min",age:"4-6",dif:"Media",obj:"Resolución grupal",mats:"Rueda de soluciones visual",lvl:1,sub:"Resolución conflictos",tags:["cooperación","razonamiento"]},
{dom:"soc",t:"Las reglas de nuestra clase",desc:"Los niños crean sus propias normas de convivencia con dibujos.",dur:"20 min",age:"4-6",dif:"Fácil",obj:"Normas sociales",mats:"Cartulina grande, pinturas",lvl:1,sub:"Normas sociales",tags:["cooperación","autonomía"]},
// ═══ MOTOR ═══
{dom:"mot",t:"Circuito psicomotor",desc:"Pasar por aros, saltar conos, gatear bajo cuerdas. Motricidad gruesa integral.",dur:"20 min",age:"3-6",dif:"Media",obj:"Motricidad gruesa",mats:"Aros, conos, cuerdas",lvl:1,sub:"M. gruesa",tags:["coordinación","equilibrio"]},
{dom:"mot",t:"Carrera de obstáculos",desc:"Circuito cronometrado con giros, saltos, arrastres. Agilidad y velocidad.",dur:"20 min",age:"4-6",dif:"Media",obj:"Agilidad",mats:"Conos, colchonetas, banco",lvl:2,sub:"M. gruesa",tags:["coordinación","velocidad"]},
{dom:"mot",t:"Coreografía grupal",desc:"Aprender una secuencia de 8-10 movimientos con música. Coordinación rítmica.",dur:"20 min",age:"4-6",dif:"Alta",obj:"Coordinación rítmica",mats:"Música y espacio",lvl:3,sub:"M. gruesa",tags:["ritmo","memoria","social"]},
{dom:"mot",t:"Taller de recortables",desc:"Recortar figuras con tijeras siguiendo líneas. Refuerza motricidad fina.",dur:"15 min",age:"4-6",dif:"Media",obj:"Motricidad fina",mats:"Tijeras punta roma, plantillas",lvl:1,sub:"M. fina",tags:["precisión","concentración"]},
{dom:"mot",t:"Ensartado de cuentas",desc:"Pasar cuentas por un cordón siguiendo un patrón de colores. Pinza fina.",dur:"15 min",age:"3-5",dif:"Fácil",obj:"Pinza digital",mats:"Cuentas grandes y cordón",lvl:1,sub:"M. fina",tags:["patrón","concentración"]},
{dom:"mot",t:"Origami básico",desc:"Plegar papel para crear figuras simples (barco, avión). Precisión manual.",dur:"15 min",age:"5-6",dif:"Media",obj:"Precisión manual",mats:"Papel cuadrado de colores",lvl:2,sub:"M. fina",tags:["concentración","instrucciones"]},
{dom:"mot",t:"Caligrafía creativa",desc:"Trazar letras y formas decorativas con diferentes grosores. Control grafomotor avanzado.",dur:"15 min",age:"5-6",dif:"Alta",obj:"Grafomotricidad",mats:"Rotuladores de diferente grosor, plantillas",lvl:3,sub:"M. fina",tags:["escritura","precisión"]},
{dom:"mot",t:"Plastilina creativa",desc:"Modelar figuras pequeñas con detalles. Fortalece manos y dedos.",dur:"15 min",age:"3-5",dif:"Fácil",obj:"Motricidad fina",mats:"Plastilina de colores",lvl:1,sub:"M. fina",tags:["creatividad","fuerza manual"]},
{dom:"mot",t:"Equilibrio sobre línea",desc:"Caminar sobre una línea recta y curva. Trabaja equilibrio y coordinación.",dur:"10 min",age:"3-6",dif:"Fácil",obj:"Equilibrio",mats:"Cinta adhesiva para suelo",lvl:1,sub:"Equilibrio",tags:["coordinación","concentración"]},
{dom:"mot",t:"Balancín y tabla",desc:"Mantenerse en tabla de equilibrio mientras atrapa pelotas suaves. Equilibrio dinámico.",dur:"10 min",age:"4-6",dif:"Media",obj:"Equilibrio dinámico",mats:"Tabla de equilibrio, pelotas suaves",lvl:2,sub:"Equilibrio",tags:["coordinación","atención"]},
{dom:"mot",t:"Yoga infantil",desc:"Posturas de yoga adaptadas: árbol, guerrero, gato. Equilibrio, fuerza y calma.",dur:"15 min",age:"3-6",dif:"Fácil",obj:"Equilibrio y calma",mats:"Colchonetas",lvl:1,sub:"Coordinación",tags:["emocional","concentración"]},
{dom:"mot",t:"Lanzar y recoger",desc:"Lanzar y atrapar una pelota con ambas manos. Coordinación óculo-manual.",dur:"10 min",age:"3-6",dif:"Fácil",obj:"Coord. óculo-manual",mats:"Pelotas blandas de diferentes tamaños",lvl:1,sub:"Lateralidad",tags:["coordinación","atención"]},
// ═══ SENSORIAL ═══
{dom:"sen",t:"Caja misteriosa táctil",desc:"Adivinar objetos con el tacto sin mirar. Estimula integración sensorial.",dur:"10 min",age:"3-6",dif:"Fácil",obj:"Integración sensorial",mats:"Caja con agujero, objetos variados",lvl:1,sub:"Táctil",tags:["discriminación","atención"]},
{dom:"sen",t:"Camino de texturas",desc:"Recorrer descalzo un camino con diferentes texturas: arena, césped, tela.",dur:"15 min",age:"3-5",dif:"Fácil",obj:"Tolerancia táctil",mats:"Bandejas con arena, arroz, telas",lvl:1,sub:"Táctil",tags:["corporal","exploración"]},
{dom:"sen",t:"Escultor ciego",desc:"Con los ojos vendados, modelar un objeto que le describe su compañero. Táctil + social.",dur:"15 min",age:"5-6",dif:"Alta",obj:"Integración avanzada",mats:"Plastilina, vendas para ojos",lvl:3,sub:"Táctil",tags:["social","comunicación","fino"]},
{dom:"sen",t:"Paisaje sonoro",desc:"Escuchar sonidos de la naturaleza e identificarlos. Procesamiento auditivo.",dur:"10 min",age:"3-5",dif:"Fácil",obj:"Proc. auditivo",mats:"Grabaciones de sonidos",lvl:1,sub:"Auditivo",tags:["atención","discriminación"]},
{dom:"sen",t:"Director de orquesta",desc:"Distinguir 4-6 instrumentos y dirigir cuál suena. Discriminación auditiva compleja.",dur:"15 min",age:"4-6",dif:"Media",obj:"Discriminación auditiva",mats:"Instrumentos musicales sencillos",lvl:2,sub:"Auditivo",tags:["atención","ritmo","social"]},
{dom:"sen",t:"Dictado de sonidos",desc:"Reproducir secuencias rítmicas de 4-6 golpes. Memoria auditiva avanzada.",dur:"10 min",age:"5-6",dif:"Alta",obj:"Memoria auditiva",mats:"Claves rítmicas o tambor",lvl:3,sub:"Auditivo",tags:["memoria","secuenciación"]},
{dom:"sen",t:"Pintura con texturas",desc:"Pintar con esponjas, hojas, telas diferentes. Exploración multisensorial.",dur:"20 min",age:"3-6",dif:"Fácil",obj:"Tolerancia sensorial",mats:"Esponjas, hojas, telas, témperas",lvl:1,sub:"Multisensorial",tags:["creatividad","táctil"]},
{dom:"sen",t:"Búsqueda visual",desc:"Encontrar objetos escondidos en una imagen o en el aula. Procesamiento visual.",dur:"10 min",age:"4-6",dif:"Media",obj:"Proc. visual",mats:"Láminas de búsqueda visual",lvl:1,sub:"Visual",tags:["atención","discriminación"]},
{dom:"sen",t:"Laberinto con los ojos",desc:"Seguir un laberinto visual sin usar el dedo. Control oculomotor y concentración.",dur:"10 min",age:"4-6",dif:"Media",obj:"Control oculomotor",mats:"Fichas de laberintos graduados",lvl:2,sub:"Visual",tags:["concentración","precisión"]},
{dom:"sen",t:"Diferencias complejas",desc:"Encontrar 8-10 diferencias entre dos imágenes detalladas. Rastreo visual avanzado.",dur:"15 min",age:"5-6",dif:"Alta",obj:"Rastreo visual",mats:"Láminas de diferencias nivel alto",lvl:3,sub:"Visual",tags:["atención","detalle"]},
{dom:"sen",t:"Cocina sensorial",desc:"Explorar ingredientes con los 5 sentidos: oler especias, tocar harinas, probar frutas.",dur:"20 min",age:"3-6",dif:"Fácil",obj:"Integración 5 sentidos",mats:"Ingredientes seguros variados",lvl:2,sub:"Multisensorial",tags:["exploración","vocabulario"]},
{dom:"sen",t:"Circuito multisensorial",desc:"Recorrido con estaciones: tocar, oler, escuchar, mirar y saborear algo en cada una.",dur:"20 min",age:"4-6",dif:"Media",obj:"Procesamiento global",mats:"5 estaciones con material sensorial",lvl:2,sub:"Propioceptivo",tags:["corporal","exploración"]},
// ═══ MOTIVACIONAL ═══
{dom:"mtv",t:"Proyecto libre semanal",desc:"Cada niño elige un mini-proyecto y lo desarrolla durante la semana.",dur:"30 min",age:"5-6",dif:"Alta",obj:"Iniciativa",mats:"Materiales variados a elección",lvl:3,sub:"Iniciativa",tags:["autonomía","creatividad"]},
{dom:"mtv",t:"Caja de propuestas",desc:"Los niños escriben/dibujan ideas de actividades para hacer en clase. Voto semanal.",dur:"10 min",age:"4-6",dif:"Fácil",obj:"Iniciativa básica",mats:"Buzón y papelitos",lvl:1,sub:"Iniciativa",tags:["participación","autonomía"]},
{dom:"mtv",t:"Líder del día",desc:"Un niño lidera una actividad que él elige y organiza para el grupo.",dur:"20 min",age:"5-6",dif:"Alta",obj:"Liderazgo",mats:"Material según actividad elegida",lvl:2,sub:"Iniciativa",tags:["social","autonomía","planificación"]},
{dom:"mtv",t:"Investigador del día",desc:"Un niño elige un tema y busca cosas sobre él para compartir.",dur:"15 min",age:"4-6",dif:"Media",obj:"Curiosidad",mats:"Libros, láminas, lupa",lvl:1,sub:"Curiosidad",tags:["lenguaje","autonomía"]},
{dom:"mtv",t:"Pregunta del día",desc:"Un niño trae cada día una pregunta curiosa. El grupo investiga la respuesta.",dur:"10 min",age:"4-6",dif:"Fácil",obj:"Pensamiento indagador",mats:"Buzón de preguntas",lvl:1,sub:"Curiosidad",tags:["lenguaje","cooperación"]},
{dom:"mtv",t:"Mini-TED",desc:"Cada niño prepara una presentación de 2 minutos sobre algo que le apasiona.",dur:"20 min",age:"5-6",dif:"Alta",obj:"Comunicación apasionada",mats:"Espacio de presentación, público",lvl:3,sub:"Curiosidad",tags:["lenguaje","autoestima","social"]},
{dom:"mtv",t:"Retos de mesa",desc:"Puzzles y desafíos con dificultad progresiva. Persistencia y foco.",dur:"15 min",age:"3-6",dif:"Media",obj:"Persistencia",mats:"Puzzles de dificultad creciente",lvl:1,sub:"Persistencia",tags:["concentración","razonamiento"]},
{dom:"mtv",t:"El error que enseña",desc:"Compartir errores del día y qué aprendió cada uno. Normalizar el fallo.",dur:"10 min",age:"4-6",dif:"Fácil",obj:"Mentalidad de crecimiento",mats:"Tablero de errores y aprendizajes",lvl:2,sub:"Persistencia",tags:["emocional","autoestima"]},
{dom:"mtv",t:"Maratón de intentos",desc:"Un reto difícil con contador de intentos visible. Celebrar cada intento, no solo el éxito.",dur:"15 min",age:"4-6",dif:"Media",obj:"Persistencia avanzada",mats:"Reto adaptado, contador visual",lvl:3,sub:"Persistencia",tags:["frustración","autocontrol"]},
{dom:"mtv",t:"Mi meta de la semana",desc:"Cada niño elige una meta personal y la trabaja toda la semana.",dur:"10 min",age:"4-6",dif:"Media",obj:"Autonomía",mats:"Tablero de metas visual",lvl:1,sub:"Autonomía",tags:["planificación","autoestima"]},
{dom:"mtv",t:"Autoevaluación diaria",desc:"El niño valora su propio día con caritas y explica por qué. Metacognición.",dur:"5 min",age:"4-6",dif:"Fácil",obj:"Metacognición",mats:"Tablero de autoevaluación con caritas",lvl:2,sub:"Autonomía",tags:["autoconciencia","reflexión"]},
{dom:"mtv",t:"Inventores del aula",desc:"Los niños inventan un juego nuevo con reglas propias. Creatividad aplicada.",dur:"30 min",age:"5-6",dif:"Alta",obj:"Creatividad aplicada",mats:"Materiales variados",lvl:2,sub:"Creatividad",tags:["cooperación","iniciativa","social"]},
{dom:"mtv",t:"Dibujo libre con tema",desc:"Dibujo libre a partir de una palabra-estímulo. Sin instrucciones específicas.",dur:"15 min",age:"3-6",dif:"Fácil",obj:"Expresión creativa",mats:"Folios y pinturas",lvl:1,sub:"Creatividad",tags:["expresión","autonomía"]}
];
const allActs=(D)=>[...ACTS,...((D||{}).customActs||[])];
const recActs=(sc,pctl,D)=>{if(!sc)return[];const weak=DOM.filter(d=>(sc[d.k]||0)<pctl.P25).map(d=>d.k);if(!weak.length){const lowest=DOM.reduce((a,d)=>(sc[d.k]||0)<(sc[a]||100)?d.k:a,DOM[0].k);weak.push(lowest);}const all=allActs(D);return all.filter(a=>weak.includes(a.dom)).slice(0,6);};
const actProgression=(act,D)=>{const all2=allActs(D);const same=all2.filter(a=>a.dom===act.dom&&a.sub===act.sub);return same.sort((a,b)=>(a.lvl||1)-(b.lvl||1));};
const nextLvlAct=(act,D)=>{const prog=actProgression(act,D);const idx=prog.findIndex(a=>a.t===act.t);return idx<prog.length-1?prog[idx+1]:null;};

function mkDemo(){return{school:{name:"Colegio Stella Maris"},users:[{id:"t1",nm:"María García",em:"maria@colegio.es",role:"teacher",pin:"1234"},{id:"p1",nm:"Dra. Marta Ruiz",em:"marta@colegio.es",role:"psico",pin:"1234"},{id:"adm1",nm:"Director Pérez",em:"admin@colegio.es",role:"admin",pin:"1234"}],parents:[{id:"pr1",nm:"Ana Torres",em:"ana@familia.es",pin:"1234",ch:["s1","s3"]},{id:"pr2",nm:"Carlos López",em:"carlos@familia.es",pin:"1234",ch:["s2"]}],cls:[{id:"cl1",name:"Aula Mariposas",grade:"P4",tid:"t1"},{id:"cl2",name:"Aula Delfines",grade:"P5",tid:"t1"}],sts:[{id:"s1",nm:"Lucía García",dob:"2021-03-15",cid:"cl1"},{id:"s2",nm:"Martín López",dob:"2021-07-22",cid:"cl1"},{id:"s3",nm:"Valentina Torres",dob:"2021-01-10",cid:"cl1"},{id:"s4",nm:"Hugo Fernández",dob:"2021-09-05",cid:"cl1"},{id:"s5",nm:"Emma Ruiz",dob:"2020-11-18",cid:"cl2"},{id:"s6",nm:"Pablo Martín",dob:"2020-04-20",cid:"cl2"},{id:"s7",nm:"Sofía Navarro",dob:"2021-05-30",cid:"cl1"},{id:"s8",nm:"Daniel Moreno",dob:"2021-02-14",cid:"cl1"}],
evs:[{id:"ev1",sid:"s1",dt:"2025-09-15",sc:{cog:72,emo:65,soc:78,mot:70,sen:68,mtv:80}},{id:"ev2",sid:"s1",dt:"2025-12-10",sc:{cog:78,emo:70,soc:82,mot:74,sen:72,mtv:85}},{id:"ev3",sid:"s2",dt:"2025-09-15",sc:{cog:60,emo:40,soc:35,mot:68,sen:60,mtv:50}},{id:"ev4",sid:"s2",dt:"2025-12-10",sc:{cog:68,emo:45,soc:38,mot:72,sen:65,mtv:55}},{id:"ev5",sid:"s3",dt:"2025-12-10",sc:{cog:85,emo:80,soc:90,mot:78,sen:82,mtv:88}},{id:"ev6",sid:"s4",dt:"2025-12-10",sc:{cog:60,emo:58,soc:65,mot:35,sen:55,mtv:50}},{id:"ev7",sid:"s5",dt:"2025-12-10",sc:{cog:88,emo:75,soc:72,mot:70,sen:78,mtv:82}},{id:"ev8",sid:"s6",dt:"2025-12-10",sc:{cog:75,emo:72,soc:78,mot:80,sen:70,mtv:85}},{id:"ev9",sid:"s7",dt:"2025-12-10",sc:{cog:62,emo:70,soc:68,mot:72,sen:60,mtv:65}},{id:"ev10",sid:"s8",dt:"2025-12-10",sc:{cog:55,emo:50,soc:58,mot:62,sen:48,mtv:52}},{id:"ev11",sid:"s1",dt:"2026-02-15",sc:{cog:82,emo:75,soc:85,mot:78,sen:76,mtv:88}},{id:"ev12",sid:"s2",dt:"2026-02-15",sc:{cog:72,emo:50,soc:42,mot:75,sen:68,mtv:58}}],
obs:[{id:"o1",sid:"s2",dt:"2025-10-05",tp:"concern",text:"Se aísla frecuentemente en el recreo. No inicia juego con pares.",dom:"soc"},{id:"o2",sid:"s4",dt:"2025-10-12",tp:"concern",text:"Dificultad notable con tijeras y punzón. Agarre inmaduro.",dom:"mot"},{id:"o3",sid:"s1",dt:"2025-11-20",tp:"positive",text:"Excelente participación en asamblea. Vocabulario amplio para su edad.",dom:"cog"},{id:"o4",sid:"s3",dt:"2025-12-01",tp:"positive",text:"Lidera juegos cooperativos con naturalidad. Gran empatía.",dom:"soc"},{id:"o5",sid:"s2",dt:"2026-01-15",tp:"neutral",text:"Hoy ha jugado 10 minutos con Pablo en el rincón de construcciones.",dom:"soc"},{id:"o6",sid:"s8",dt:"2026-01-20",tp:"concern",text:"Dificultad para mantener atención en la asamblea. Se distrae fácilmente.",dom:"cog"},{id:"o7",sid:"s4",dt:"2026-02-01",tp:"positive",text:"Ha mejorado el recortado de figuras simples con apoyo.",dom:"mot"},{id:"o8",sid:"s7",dt:"2026-02-05",tp:"neutral",text:"Participación irregular. Algunos días muy activa, otros muy callada.",dom:"emo"}],
als:[{id:"al1",sid:"s2",dom:"soc",msg:"Social por debajo del P10 — Derivación recomendada",st:"active",sev:"high"},{id:"al2",sid:"s4",dom:"mot",msg:"Motricidad fina por debajo del P10",st:"active",sev:"high"},{id:"al3",sid:"s2",dom:"emo",msg:"Emocional por debajo del P25",st:"active",sev:"medium"},{id:"al4",sid:"s4",dom:"mtv",msg:"Motivacional por debajo del P25",st:"active",sev:"medium"},{id:"al5",sid:"s8",dom:"sen",msg:"Sensorial por debajo del P25 — Seguimiento",st:"active",sev:"medium"},{id:"al6",sid:"s8",dom:"cog",msg:"Cognitivo por debajo del P25",st:"active",sev:"medium"}],
pmi:[{id:"pm1",sid:"s2",dom:"soc",obj:["1 juego grupal diario con compañero-guía","Teatro de roles 2x/semana","Asignar responsabilidad grupal"],status:"active",pr:35,vby:null,dt:"2025-10-20",rev:"2026-03-20"},{id:"pm2",sid:"s4",dom:"mot",obj:["Ejercicios de pinza 10 min/día","Circuito psicomotor 2x/semana","Plastilina con detalles finos"],status:"active",pr:20,vby:null,dt:"2025-10-25",rev:"2026-03-25"},{id:"pm3",sid:"s8",dom:"cog",obj:["Juegos de memoria 15 min/día","Lectura compartida con preguntas","Secuencias lógicas progresivas"],status:"active",pr:10,vby:null,dt:"2026-01-25",rev:"2026-04-25"}],
msgs:[{id:"m1",sid:"s1",from:"t1",to:"pr1",dt:"2025-12-01",text:"Lucía progresa muy bien en todas las áreas. Su evolución en lenguaje es notable."},{id:"m2",sid:"s2",from:"t1",to:"pr2",dt:"2025-12-05",text:"Martín necesita más apoyo en socialización. ¿Podemos concertar una tutoría?"},{id:"m3",sid:"s1",from:"pr1",to:"t1",dt:"2025-12-02",text:"Gracias María, en casa también la vemos muy contenta y motivada."},{id:"m4",sid:"s2",from:"pr2",to:"t1",dt:"2025-12-06",text:"Sí, por supuesto. ¿Qué día le vendría bien?"},{id:"m5",sid:"s3",from:"t1",to:"pr1",dt:"2026-01-15",text:"Valentina sigue siendo un referente positivo en el aula. Excelente evolución social."},{id:"m6",sid:"s2",from:"t1",to:"pr2",dt:"2026-02-10",text:"Martín ha mostrado pequeños avances en juego social. Seguimos trabajando."}],
journal:[{id:"j1",dt:"2026-02-20",mood:"positive",text:"Excelente jornada. Asamblea sobre emociones muy participativa. Martín se integró en el juego de construcciones cooperativas. Hugo mejoró en recortables.",climate:"Tranquilo",acts:["El semáforo emocional","Construcción cooperativa"],attendance:8},{id:"j2",dt:"2026-02-19",mood:"neutral",text:"Día normal. Sofía algo retraída hoy. Daniel necesitó apoyo extra en atención. Lucía brillante en el cuentacuentos.",climate:"Normal",acts:["Cuentacuentos con preguntas","Plastilina creativa"],attendance:7},{id:"j3",dt:"2026-02-18",mood:"positive",text:"Gran día de psicomotricidad. Todos participaron activamente en el circuito. Hugo mostró mejor equilibrio.",climate:"Activo",acts:["Circuito psicomotor","Equilibrio sobre línea"],attendance:8},{id:"j4",dt:"2026-02-17",mood:"concern",text:"Día complicado. Varios conflictos en el recreo. Martín volvió a aislarse. Hablar con familia de Daniel por falta de atención recurrente.",climate:"Agitado",acts:["Teatro de roles"],attendance:8},{id:"j5",dt:"2026-02-14",mood:"positive",text:"Fiesta de San Valentín escolar. Actividades de amistad y cooperación. Muy buen clima emocional.",climate:"Excelente",acts:["El juego del espejo","Diario de emociones"],attendance:8}],
pNotes:[{id:"pn1",sid:"s2",dt:"2025-12-05",text:"Recomiendo evaluación de habilidades sociales con especialista. Posible TEA leve a descartar.",from:"t1",type:"derivacion"},{id:"pn2",sid:"s4",dt:"2025-12-06",text:"Derivar a terapia ocupacional para motricidad fina. Agarre inmaduro para su edad.",from:"t1",type:"derivacion"},{id:"pn3",sid:"s1",dt:"2026-02-15",text:"Lucía evoluciona muy bien. Compartir logros con familia. Recomendar actividades de enriquecimiento cognitivo.",from:"t1",type:"info"},{id:"pn4",sid:"s8",dt:"2026-01-25",text:"Daniel muestra dificultades de atención sostenida. Iniciar plan de refuerzo cognitivo. Informar familia.",from:"t1",type:"seguimiento"},{id:"pn5",sid:"s2",dt:"2026-02-10",text:"Martín muestra pequeños avances en juego social dirigido. Mantener estrategias actuales.",from:"t1",type:"seguimiento"}],
psNotes:[],hDiary:[],
milestones:[{id:"ms1",sid:"s1",dt:"2025-11-15",text:"Primera lectura autónoma de palabras simples",dom:"cog"},{id:"ms2",sid:"s3",dt:"2025-12-01",text:"Resuelve conflictos entre compañeros sin ayuda del adulto",dom:"soc"},{id:"ms3",sid:"s2",dt:"2026-01-20",text:"Primer juego cooperativo sostenido (10 min)",dom:"soc"},{id:"ms4",sid:"s4",dt:"2026-02-01",text:"Recorta figuras simples por la línea con apoyo mínimo",dom:"mot"},{id:"ms5",sid:"s1",dt:"2026-02-10",text:"Cuenta hasta 20 y reconoce números escritos",dom:"cog"},{id:"ms6",sid:"s5",dt:"2026-01-15",text:"Lee frases cortas de forma autónoma",dom:"cog"}],
dailyAn:[{dt:"2026-02-20",focus:"Emocional y Social",summary:"Clima positivo. Actividades de regulación funcionaron bien. Martín conectó con grupo brevemente.",highlights:["Martín jugó en grupo 15 min","Hugo mejoró recorte","Lucía vocabulario excepcional"],concerns:["Sofía intermitente en participación","Daniel distracción en asamblea"],domFocus:{emo:72,soc:68}},{dt:"2026-02-19",focus:"Cognitivo y Motor",summary:"Jornada estándar. Buena respuesta a cuentacuentos. Taller de plastilina mostró diferencias de nivel.",highlights:["Valentina lideró narración","Emma lectura avanzada"],concerns:["Daniel atención <5 min en lectura","Hugo agarre aún inmaduro"],domFocus:{cog:70,mot:62}},{dt:"2026-02-18",focus:"Motor",summary:"Sesión de psicomotricidad muy positiva. Mejoras visibles en equilibrio general del grupo.",highlights:["Hugo mejor equilibrio","Todos completaron circuito"],concerns:["Martín evitó actividad grupal inicial"],domFocus:{mot:74}}],
actAssign:[{id:"aa1",sid:"s2",act:"Construcción cooperativa",dom:"soc",dt:"2026-02-10",done:true,notes:"Participó 15 min con apoyo"},{id:"aa2",sid:"s2",act:"Teatro de roles",dom:"soc",dt:"2026-02-12",done:true,notes:"Representó papel de comprador en tienda"},{id:"aa3",sid:"s2",act:"El juego del espejo",dom:"soc",dt:"2026-02-18",done:false,notes:""},{id:"aa4",sid:"s4",act:"Taller de recortables",dom:"mot",dt:"2026-02-10",done:true,notes:"Mejoró en líneas rectas"},{id:"aa5",sid:"s4",act:"Plastilina creativa",dom:"mot",dt:"2026-02-15",done:true,notes:"Hizo figura de perro con detalles"},{id:"aa6",sid:"s4",act:"Circuito psicomotor",dom:"mot",dt:"2026-02-18",done:true,notes:"Completó todo el circuito sin ayuda"},{id:"aa7",sid:"s8",act:"Memory con tarjetas",dom:"cog",dt:"2026-02-12",done:true,notes:"Encontró 6 de 8 parejas"},{id:"aa8",sid:"s8",act:"Cuentacuentos con preguntas",dom:"cog",dt:"2026-02-15",done:false,notes:""},{id:"aa9",sid:"s1",act:"Secuencias lógicas",dom:"cog",dt:"2026-02-14",done:true,notes:"Completó nivel avanzado sin ayuda"}],
incidents:[{id:"inc1",sid:"s2",dt:"2025-11-10",type:"aislamiento",text:"Se sentó solo durante toda la hora del patio. No respondió a invitaciones de compañeros.",sev:"medium"},{id:"inc2",sid:"s4",dt:"2025-12-03",type:"frustracion",text:"Rompió a llorar al no poder recortar una figura. Se negó a seguir intentándolo.",sev:"low"},{id:"inc3",sid:"s8",dt:"2026-01-18",type:"atencion",text:"Se levantó 6 veces durante la asamblea de 20 min. No pudo completar la actividad dirigida.",sev:"medium"},{id:"inc4",sid:"s2",dt:"2026-02-04",type:"conflicto",text:"Pequeño conflicto verbal con Pablo por un juguete. Se resolvió con mediación.",sev:"low"}],
invitations:[{id:"inv1",code:"MAR-2025",sid:"s1",tid:"t1",parId:"pr1",dt:"2025-09-01",status:"accepted"},{id:"inv2",code:"MAR-2026",sid:"s3",tid:"t1",parId:"pr1",dt:"2025-09-01",status:"accepted"},{id:"inv3",code:"LOP-2025",sid:"s2",tid:"t1",parId:"pr2",dt:"2025-09-01",status:"accepted"},{id:"inv4",code:"FER-2025",sid:"s4",tid:"t1",parId:null,dt:"2026-02-15",status:"pending"},{id:"inv5",code:"NAV-2025",sid:"s7",tid:"t1",parId:null,dt:"2026-02-18",status:"pending"}],
guidedEvals:[]};}

/* ── UI Components ── */
function Av({nm,sz}){const n=nm||"?";const t=n.split(" ").map(w=>(w||"")[0]).join("").slice(0,2).toUpperCase();const cs=["#0D9488","#3B82F6","#8B5CF6","#EC4899","#F59E0B","#10B981"];const c=cs[n.charCodeAt(0)%cs.length];const s=sz||36;return <div style={{width:s,height:s,borderRadius:99,background:c+"15",display:"flex",alignItems:"center",justifyContent:"center",fontSize:s*.33,fontWeight:700,color:c,flexShrink:0}}>{t}</div>;}
function NLogo({sz}){const s=sz||36;return <div style={{width:s,height:s,borderRadius:s*.28,background:"linear-gradient(135deg,"+P+","+PD+")",display:"flex",alignItems:"center",justifyContent:"center",color:"#fff",fontWeight:800,fontSize:s*.4,fontFamily:FD}}>N</div>;}
function Pill({t,co,bg}){return <span style={{display:"inline-block",padding:"3px 9px",borderRadius:99,fontSize:10,fontWeight:600,color:co||P,background:bg||PL,fontFamily:F}}>{t}</span>;}
function Bars({sc,sm,pctl}){return <div style={{display:"flex",flexDirection:"column",gap:sm?4:6}}>{DOM.map(d=>{const v=sc[d.k]||0;const Ic=d.I;const p50=pctl?.P50||65;return <div key={d.k}><div style={{display:"flex",justifyContent:"space-between",fontSize:sm?11:12,marginBottom:2}}><span style={{display:"flex",alignItems:"center",gap:4}}><Ic size={sm?11:13} color={d.c}/>{d.l}</span><span style={{fontWeight:700,color:percCo(v)}}>{v}%</span></div><div style={{height:sm?4:5,borderRadius:99,background:BL,position:"relative"}}><div style={{height:"100%",borderRadius:99,background:d.c,width:v+"%"}}/>{!sm&&pctl&&<div style={{position:"absolute",top:-1,left:p50+"%",width:2,height:sm?6:7,background:TL,borderRadius:1}} title={"P50: "+p50}/>}</div></div>})}</div>;}
function Mdl({title,onClose,children,wide}){return <div style={{position:"fixed",inset:0,background:"rgba(15,23,42,.45)",display:"flex",alignItems:"center",justifyContent:"center",zIndex:1000,padding:16}} onClick={onClose}><div style={{background:"#fff",borderRadius:18,maxWidth:wide?700:500,width:"100%",maxHeight:"90vh",overflow:"auto"}} onClick={e=>e.stopPropagation()}><div style={{padding:"16px 20px",borderBottom:"1px solid "+BD,display:"flex",justifyContent:"space-between",alignItems:"center"}}><h3 style={{fontFamily:FD,fontSize:17,margin:0}}>{title}</h3><button onClick={onClose} style={{width:30,height:30,borderRadius:8,border:"none",background:BL,cursor:"pointer",color:TM}}>✕</button></div><div style={{padding:20}}>{children}</div></div></div>;}

/* ── Top-level Modals ── */
function EvalModal({student,D,up,onClose}){
  const [step,setStep]=useState(0);
  const [sc,setSc]=useState({});
  if(!student)return null;
  const dk=DOM[step].k;const its=ITEMS[dk];const Ic=DOM[step].I;
  const save=()=>{const r={};DOM.forEach(d=>{const it=ITEMS[d.k];r[d.k]=it.length?Math.round(it.reduce((a,_,i)=>a+(sc[d.k+"_"+i]||0),0)/it.length):0;});const newAls=[...(D.als||[])];const pctl=getPctl(student.dob);DOM.forEach(d=>{if(r[d.k]<pctl.P25&&!newAls.find(a=>a.sid===student.id&&a.dom===d.k&&a.st==="active")){const sev=r[d.k]<pctl.P10?"high":"medium";newAls.push({id:uid(),sid:student.id,dom:d.k,msg:d.l+(sev==="high"?" bajo P10 — Derivación":" bajo P25"),st:"active",sev});}});up({evs:[...(D.evs||[]),{id:uid(),sid:student.id,dt:td(),sc:r}],als:newAls});onClose();};
  return <Mdl title={"Evaluar: "+student.nm} onClose={onClose}><div>
    <div style={{display:"flex",gap:3,marginBottom:10}}>{DOM.map((_,i)=><div key={i} style={{flex:1,height:3,borderRadius:99,background:i<step?P:i===step?"#14B8A6":BL}}/>)}</div>
    <div style={{display:"flex",alignItems:"center",gap:5,marginBottom:10}}><Ic size={17} color={DOM[step].c}/><span style={{fontSize:14,fontWeight:600,color:DOM[step].c}}>{DOM[step].l} ({step+1}/6)</span></div>
    {its.map((it,i)=><div key={i} style={{marginBottom:9}}><div style={{fontSize:13,fontWeight:500,marginBottom:4}}>{it}</div><div style={{display:"flex",gap:4}}>{[{v:0,l:"No obs."},{v:25,l:"Rara vez"},{v:50,l:"A veces"},{v:75,l:"Frecuente"},{v:100,l:"Siempre"}].map(val=><button key={val.v} onClick={()=>setSc({...sc,[dk+"_"+i]:val.v})} style={{flex:1,padding:6,borderRadius:8,border:sc[dk+"_"+i]===val.v?"2px solid "+DOM[step].c:"2px solid "+BD,background:sc[dk+"_"+i]===val.v?DOM[step].c+"12":"#fff",color:sc[dk+"_"+i]===val.v?DOM[step].c:TM,fontSize:10,cursor:"pointer",fontFamily:F,fontWeight:sc[dk+"_"+i]===val.v?700:400}}>{val.l}</button>)}</div></div>)}
    <div style={{display:"flex",gap:6,marginTop:12}}>{step>0&&<button onClick={()=>setStep(step-1)} style={{...bo(false),flex:1}}>Atrás</button>}{step<5?<button onClick={()=>setStep(step+1)} style={{...bp(),flex:1}}>Siguiente</button>:<button onClick={save} style={{...bp(),flex:1}}>Guardar evaluación</button>}</div>
  </div></Mdl>;
}
function AddClsModal({authUid,D,up,onClose}){
  const [n,setN]=useState("");const [g,setG]=useState("P4");
  const save=()=>{if(!n.trim())return;up({cls:[...(D.cls||[]),{id:uid(),name:n.trim(),grade:g,tid:authUid}]});onClose();};
  return <Mdl title="Nueva aula" onClose={onClose}><div><input value={n} onChange={e=>setN(e.target.value)} placeholder="Nombre del aula" style={{...inp,marginBottom:8}}/><select value={g} onChange={e=>setG(e.target.value)} style={{...inp,marginBottom:10}}>{["P2","P3","P4","P5","1º","2º","3º","4º","5º","6º"].map(x=><option key={x}>{x}</option>)}</select><button onClick={save} disabled={!n.trim()} style={{...bp(),width:"100%",opacity:n.trim()?1:.5}}>Crear</button></div></Mdl>;
}
function AddStModal({clsId,D,up,onClose}){
  const [n,setN]=useState("");const [dob,setDob]=useState("2021-01-01");
  const save=()=>{if(!n.trim()||!clsId)return;up({sts:[...(D.sts||[]),{id:uid(),nm:n.trim(),dob,cid:clsId}]});onClose();};
  return <Mdl title="Nuevo alumno" onClose={onClose}><div><input value={n} onChange={e=>setN(e.target.value)} placeholder="Nombre completo" style={{...inp,marginBottom:8}}/><label style={{fontSize:12,color:TM}}>Fecha nacimiento</label><input type="date" value={dob} onChange={e=>setDob(e.target.value)} style={{...inp,marginBottom:10}}/><button onClick={save} disabled={!n.trim()} style={{...bp(),width:"100%",opacity:n.trim()?1:.5}}>Añadir</button></div></Mdl>;
}
function ObsModal({student,D,up,onClose}){
  const [t,setT]=useState("");const [tp,setTp]=useState("neutral");const [dm,setDm]=useState("cog");
  if(!student)return null;
  const save=()=>{if(!t.trim())return;up({obs:[...(D.obs||[]),{id:uid(),sid:student.id,dt:td(),tp,text:t.trim(),dom:dm}]});onClose();};
  return <Mdl title={"Observación: "+student.nm} onClose={onClose}><div>
    <div style={{display:"flex",gap:4,marginBottom:8}}>{[{k:"positive",l:"Positiva",c:"#059669"},{k:"neutral",l:"Neutral",c:TM},{k:"concern",l:"Preocupante",c:"#D97706"}].map(x=><button key={x.k} onClick={()=>setTp(x.k)} style={{...bo(tp===x.k,true),color:tp===x.k?x.c:TM,borderColor:tp===x.k?x.c:BD}}>{x.l}</button>)}</div>
    <select value={dm} onChange={e=>setDm(e.target.value)} style={{...inp,marginBottom:8}}>{DOM.map(d=><option key={d.k} value={d.k}>{d.l}</option>)}</select>
    <textarea value={t} onChange={e=>setT(e.target.value)} placeholder="Describe la observación..." rows={4} style={{...inp,resize:"vertical"}}/><button onClick={save} disabled={!t.trim()} style={{...bp(),width:"100%",marginTop:8,opacity:t.trim()?1:.5}}>Guardar</button>
  </div></Mdl>;
}
function JournalModal({D,up,onClose}){
  const [t,setT]=useState("");const [mood,setMood]=useState("positive");const [cl,setCl]=useState("Normal");const [att,setAtt]=useState(8);const [acts,setActs]=useState([]);
  const save=()=>{if(!t.trim())return;up({journal:[{id:uid(),dt:td(),mood,text:t.trim(),climate:cl,acts,attendance:parseInt(att)||0},...(D.journal||[])]});onClose();};
  return <Mdl title="Nuevo registro diario" onClose={onClose} wide><div>
    <div style={{display:"flex",gap:4,marginBottom:8}}>{[{k:"positive",l:"Positivo"},{k:"neutral",l:"Neutral"},{k:"concern",l:"Preocupante"}].map(x=><button key={x.k} onClick={()=>setMood(x.k)} style={bo(mood===x.k,true)}>{x.l}</button>)}</div>
    <div style={{display:"flex",gap:6,marginBottom:8}}><div style={{flex:1}}><label style={{fontSize:11,color:TM}}>Clima</label><select value={cl} onChange={e=>setCl(e.target.value)} style={inp}>{["Excelente","Tranquilo","Normal","Activo","Agitado","Difícil"].map(x=><option key={x}>{x}</option>)}</select></div><div style={{flex:1}}><label style={{fontSize:11,color:TM}}>Asistencia</label><input type="number" value={att} onChange={e=>setAtt(e.target.value)} style={inp} min={0}/></div></div>
    <textarea value={t} onChange={e=>setT(e.target.value)} placeholder="Describe la jornada..." rows={4} style={{...inp,marginBottom:8,resize:"vertical"}}/>
    <div style={{marginBottom:8}}><label style={{fontSize:11,color:TM}}>Actividades realizadas (selecciona)</label><div style={{display:"flex",gap:4,flexWrap:"wrap",marginTop:4}}>{ACTS.slice(0,12).map((a,i)=><button key={i} onClick={()=>setActs(acts.includes(a.t)?acts.filter(x=>x!==a.t):[...acts,a.t])} style={{...bo(acts.includes(a.t),true),fontSize:10}}>{a.t}</button>)}</div></div>
    <button onClick={save} disabled={!t.trim()} style={{...bp(),width:"100%",opacity:t.trim()?1:.5}}>Guardar</button>
  </div></Mdl>;
}
function PNoteModal({student,D,up,authUid,onClose}){
  const [t,setT]=useState("");const [tp,setTp]=useState("info");
  if(!student)return null;
  const save=()=>{if(!t.trim())return;up({pNotes:[...(D.pNotes||[]),{id:uid(),sid:student.id,dt:td(),text:t.trim(),from:authUid,type:tp}]});onClose();};
  return <Mdl title={"Nota para familia: "+student.nm} onClose={onClose}><div>
    <div style={{display:"flex",gap:4,marginBottom:8}}>{[{k:"info",l:"Informativa",c:P},{k:"seguimiento",l:"Seguimiento",c:"#D97706"},{k:"derivacion",l:"Derivación",c:"#DC2626"},{k:"reunion",l:"Reunión",c:"#3B82F6"},{k:"felicitacion",l:"Felicitación",c:"#059669"}].map(x=><button key={x.k} onClick={()=>setTp(x.k)} style={{...bo(tp===x.k,true),color:tp===x.k?x.c:TM,borderColor:tp===x.k?x.c:BD,fontSize:10}}>{x.l}</button>)}</div>
    <textarea value={t} onChange={e=>setT(e.target.value)} placeholder="Escribe la nota..." rows={4} style={{...inp,resize:"vertical"}}/>
    <button onClick={save} disabled={!t.trim()} style={{...bp(),width:"100%",marginTop:8,opacity:t.trim()?1:.5}}>Enviar</button>
  </div></Mdl>;
}
function ActDetailModal({act,onClose}){
  if(!act)return null;const d=DOM.find(x=>x.k===act.dom);const Ic=d?.I;
  return <Mdl title={act.t} onClose={onClose}><div>
    <div style={{display:"flex",alignItems:"center",gap:6,marginBottom:10}}>{Ic&&<div style={{width:36,height:36,borderRadius:10,background:d.c+"15",display:"flex",alignItems:"center",justifyContent:"center"}}><Ic size={18} color={d.c}/></div>}<div><div style={{fontSize:14,fontWeight:600}}>{act.t}</div><div style={{fontSize:12,color:d?.c}}>{d?.l} · {act.obj}</div></div></div>
    <p style={{fontSize:13,color:TX,lineHeight:1.6,margin:"0 0 12px"}}>{act.desc}</p>
    <div style={{display:"grid",gridTemplateColumns:"1fr 1fr 1fr",gap:6}}>{[{l:"Duración",v:act.dur},{l:"Edad",v:act.age+" años"},{l:"Dificultad",v:act.dif}].map((x,i)=><div key={i} style={{padding:8,background:BL,borderRadius:8,textAlign:"center"}}><div style={{fontSize:10,color:TM}}>{x.l}</div><div style={{fontSize:12,fontWeight:600}}>{x.v}</div></div>)}</div>
    {act.mats&&<div style={{marginTop:10,padding:8,background:PL,borderRadius:8}}><div style={{fontSize:10,fontWeight:600,color:PD,marginBottom:2}}>Materiales necesarios</div><div style={{fontSize:12,color:TX}}>{act.mats}</div></div>}
  </div></Mdl>;
}
/* New: Assign Activity Modal */
function AssignActModal({student,D,up,onClose}){
  const [selAct,setSelAct]=useState(null);const [fDom,setFDom]=useState("all");
  if(!student)return null;
  const fActs=fDom==="all"?allActs(D):allActs(D).filter(a=>a.dom===fDom);
  const save=()=>{if(!selAct)return;up({actAssign:[...(D.actAssign||[]),{id:uid(),sid:student.id,act:selAct.t,dom:selAct.dom,dt:td(),done:false,notes:""}]});onClose();};
  return <Mdl title={"Asignar actividad: "+student.nm} onClose={onClose} wide><div>
    <div style={{display:"flex",gap:3,marginBottom:8,flexWrap:"wrap"}}><button onClick={()=>setFDom("all")} style={bo(fDom==="all",true)}>Todas</button>{DOM.map(d=><button key={d.k} onClick={()=>setFDom(d.k)} style={bo(fDom===d.k,true)}>{d.l}</button>)}</div>
    <div style={{maxHeight:300,overflowY:"auto"}}>{fActs.map((a,i)=>{const d=DOM.find(x=>x.k===a.dom);return <div key={i} onClick={()=>setSelAct(a)} style={{display:"flex",alignItems:"center",gap:8,padding:8,borderRadius:8,cursor:"pointer",background:selAct?.t===a.t?PL:"transparent",border:selAct?.t===a.t?"1.5px solid "+P:"1.5px solid transparent",marginBottom:3}}><div style={{width:24,height:24,borderRadius:6,background:d?.c+"15",display:"flex",alignItems:"center",justifyContent:"center"}}>{d?.I&&<d.I size={12} color={d.c}/>}</div><div style={{flex:1}}><div style={{fontSize:12,fontWeight:500}}>{a.t}</div><div style={{fontSize:10,color:TM}}>{a.obj} · {a.dur}</div></div></div>})}</div>
    <button onClick={save} disabled={!selAct} style={{...bp(),width:"100%",marginTop:10,opacity:selAct?1:.5}}>Asignar actividad</button>
  </div></Mdl>;
}
/* New: PMI Creation Modal */
function PMIModal({student,D,up,onClose}){
  const [dm,setDm]=useState("cog");const [o1,setO1]=useState("");const [o2,setO2]=useState("");const [o3,setO3]=useState("");const [showRecs,setShowRecs]=useState(true);
  if(!student)return null;
  const ageR=getAgeRange(student.dob);const recs=(PMI_RECS[dm]||{})[ageR]||[];
  const addRec=(txt)=>{if(!o1.trim())setO1(txt);else if(!o2.trim())setO2(txt);else if(!o3.trim())setO3(txt);};
  const usedRecs=[o1,o2,o3];
  const save=()=>{const objs=[o1,o2,o3].filter(x=>x.trim());if(!objs.length)return;up({pmi:[...(D.pmi||[]),{id:uid(),sid:student.id,dom:dm,obj:objs,status:"active",pr:0,vby:null,dt:td(),rev:""}]});onClose();};
  return <Mdl title={"Nuevo PMI: "+student.nm} onClose={onClose} wide><div>
    <div style={{display:"flex",alignItems:"center",gap:6,marginBottom:8}}><span style={{fontSize:11,color:TM}}>Edad: <strong style={{color:TX}}>{ageFn(student.dob)}</strong></span><span style={{fontSize:11,color:TM}}>· Rango: <strong style={{color:P}}>{ageR} años</strong></span></div>
    <label style={{fontSize:12,color:TM}}>Dominio</label>
    <div style={{display:"flex",gap:3,marginBottom:10,flexWrap:"wrap"}}>{DOM.map(d=>{const Ic=d.I;return <button key={d.k} onClick={()=>setDm(d.k)} style={{...bo(dm===d.k,true),display:"flex",alignItems:"center",gap:4,color:dm===d.k?"#fff":d.c,background:dm===d.k?d.c:d.c+"12",borderColor:d.c}}><Ic size={11}/>{d.l}</button>})}</div>
    <label style={{fontSize:12,color:TM}}>Objetivos (mínimo 1)</label>
    <input value={o1} onChange={e=>setO1(e.target.value)} placeholder="Objetivo 1" style={{...inp,marginBottom:4}}/>
    <input value={o2} onChange={e=>setO2(e.target.value)} placeholder="Objetivo 2 (opcional)" style={{...inp,marginBottom:4}}/>
    <input value={o3} onChange={e=>setO3(e.target.value)} placeholder="Objetivo 3 (opcional)" style={{...inp,marginBottom:6}}/>
    {recs.length>0&&<div style={{marginBottom:10}}>
      <div style={{display:"flex",justifyContent:"space-between",alignItems:"center",marginBottom:6}}><div style={{fontSize:11,fontWeight:600,color:P}}>Sugerencias para {ageR} años — {DOM.find(d=>d.k===dm)?.l}</div><button onClick={()=>setShowRecs(!showRecs)} style={{fontSize:10,color:TM,background:"none",border:"none",cursor:"pointer",textDecoration:"underline"}}>{showRecs?"Ocultar":"Mostrar"}</button></div>
      {showRecs&&<div style={{display:"flex",flexDirection:"column",gap:3}}>{recs.map((r,i)=>{const used=usedRecs.includes(r);return <button key={i} onClick={()=>!used&&addRec(r)} style={{display:"flex",alignItems:"center",gap:6,padding:"7px 10px",borderRadius:8,border:"1px solid "+(used?P+"40":BL),background:used?PL+"80":"#fff",cursor:used?"default":"pointer",textAlign:"left",fontFamily:F,fontSize:11,color:used?PD:TX,transition:"all .15s"}}><span style={{width:18,height:18,borderRadius:99,background:used?P:BL,color:used?"#fff":TM,display:"flex",alignItems:"center",justifyContent:"center",fontSize:9,fontWeight:700,flexShrink:0}}>{used?"✓":"+"}</span><span>{r}</span></button>})}</div>}
      <div style={{fontSize:9,color:TL,marginTop:4,fontStyle:"italic"}}>Haz clic en una sugerencia para añadirla como objetivo. Puedes editarla o escribir tus propios objetivos.</div>
    </div>}
    <button onClick={save} disabled={!o1.trim()} style={{...bp(),width:"100%",opacity:o1.trim()?1:.5}}>Crear PMI</button>
  </div></Mdl>;
}
/* New: Milestone Modal */
function MilestoneModal({student,D,up,onClose}){
  const [t,setT]=useState("");const [dm,setDm]=useState("cog");const [showRecs,setShowRecs]=useState(true);
  if(!student)return null;
  const ageR=getAgeRange(student.dob);const recs=((MILE_RECS[ageR]||{})[dm])||[];
  const existingMiles=(D.milestones||[]).filter(m=>m.sid===student.id).map(m=>m.text);
  const save=()=>{if(!t.trim())return;up({milestones:[...(D.milestones||[]),{id:uid(),sid:student.id,dt:td(),text:t.trim(),dom:dm}]});onClose();};
  return <Mdl title={"Nuevo hito: "+student.nm} onClose={onClose} wide><div>
    <div style={{display:"flex",alignItems:"center",gap:6,marginBottom:8}}><span style={{fontSize:11,color:TM}}>Edad: <strong style={{color:TX}}>{ageFn(student.dob)}</strong></span><span style={{fontSize:11,color:TM}}>· Rango: <strong style={{color:P}}>{ageR} años</strong></span></div>
    <label style={{fontSize:12,color:TM}}>Dominio del hito</label>
    <div style={{display:"flex",gap:3,marginBottom:8,flexWrap:"wrap"}}>{DOM.map(d=>{const Ic=d.I;return <button key={d.k} onClick={()=>setDm(d.k)} style={{...bo(dm===d.k,true),display:"flex",alignItems:"center",gap:4,color:dm===d.k?"#fff":d.c,background:dm===d.k?d.c:d.c+"12",borderColor:d.c}}><Ic size={11}/>{d.l}</button>})}</div>
    <textarea value={t} onChange={e=>setT(e.target.value)} placeholder="Describe el hito alcanzado..." rows={2} style={{...inp,resize:"vertical",marginBottom:6}}/>
    {recs.length>0&&<div style={{marginBottom:8}}>
      <div style={{display:"flex",justifyContent:"space-between",alignItems:"center",marginBottom:6}}><div style={{display:"flex",alignItems:"center",gap:4}}><span style={{fontSize:14}}>🌟</span><span style={{fontSize:11,fontWeight:600,color:"#D97706"}}>Hitos típicos {ageR} años — {DOM.find(d=>d.k===dm)?.l}</span></div><button onClick={()=>setShowRecs(!showRecs)} style={{fontSize:10,color:TM,background:"none",border:"none",cursor:"pointer",textDecoration:"underline"}}>{showRecs?"Ocultar":"Mostrar"}</button></div>
      {showRecs&&<div style={{display:"flex",flexDirection:"column",gap:3}}>{recs.map((r,i)=>{const already=existingMiles.includes(r);return <button key={i} onClick={()=>!already&&setT(r)} disabled={already} style={{display:"flex",alignItems:"center",gap:6,padding:"7px 10px",borderRadius:8,border:"1px solid "+(t===r?"#F59E0B40":already?PL:BL),background:t===r?"#FFFBEB":already?BG:"#fff",cursor:already?"default":"pointer",textAlign:"left",fontFamily:F,fontSize:11,color:already?TL:t===r?"#B45309":TX,transition:"all .15s",opacity:already?.5:1}}><span style={{width:18,height:18,borderRadius:99,background:already?P:t===r?"#F59E0B":BL,color:already||t===r?"#fff":TM,display:"flex",alignItems:"center",justifyContent:"center",fontSize:9,fontWeight:700,flexShrink:0}}>{already?"✓":t===r?"★":"+"}</span><span style={{textDecoration:already?"line-through":"none"}}>{r}</span>{already&&<span style={{fontSize:8,color:TL,marginLeft:"auto"}}>ya registrado</span>}</button>})}</div>}
      <div style={{fontSize:9,color:TL,marginTop:4,fontStyle:"italic"}}>Selecciona un hito sugerido o escribe uno propio. Los ya registrados aparecen tachados.</div>
    </div>}
    <button onClick={save} disabled={!t.trim()} style={{...bp(),width:"100%",opacity:t.trim()?1:.5}}>Registrar hito</button>
  </div></Mdl>;
}
/* New: Incident Modal */
function IncidentModal({student,D,up,onClose}){
  const [t,setT]=useState("");const [tp,setTp]=useState("atencion");const [sv,setSv]=useState("low");
  if(!student)return null;
  const save=()=>{if(!t.trim())return;up({incidents:[...(D.incidents||[]),{id:uid(),sid:student.id,dt:td(),type:tp,text:t.trim(),sev:sv}]});onClose();};
  return <Mdl title={"Incidencia: "+student.nm} onClose={onClose}><div>
    <label style={{fontSize:12,color:TM}}>Tipo</label>
    <div style={{display:"flex",gap:4,marginBottom:8}}>{[{k:"atencion",l:"Atención"},{k:"conducta",l:"Conducta"},{k:"frustracion",l:"Frustración"},{k:"conflicto",l:"Conflicto"},{k:"aislamiento",l:"Aislamiento"}].map(x=><button key={x.k} onClick={()=>setTp(x.k)} style={bo(tp===x.k,true)}>{x.l}</button>)}</div>
    <label style={{fontSize:12,color:TM}}>Severidad</label>
    <div style={{display:"flex",gap:4,marginBottom:8}}>{[{k:"low",l:"Baja",c:"#D97706"},{k:"medium",l:"Media",c:"#EA580C"},{k:"high",l:"Alta",c:"#DC2626"}].map(x=><button key={x.k} onClick={()=>setSv(x.k)} style={{...bo(sv===x.k,true),color:sv===x.k?x.c:TM}}>{x.l}</button>)}</div>
    <textarea value={t} onChange={e=>setT(e.target.value)} placeholder="Describe la incidencia..." rows={3} style={{...inp,resize:"vertical"}}/>
    <button onClick={save} disabled={!t.trim()} style={{...bp(),width:"100%",marginTop:8,opacity:t.trim()?1:.5}}>Registrar</button>
  </div></Mdl>;
}
/* New: Invite Parent Modal */
function InviteParentModal({student,D,up,authUid,onClose}){
  const [code,setCode]=useState(()=>{const nm=(student?.nm||"X").split(" ");return (nm[nm.length-1]||"X").slice(0,3).toUpperCase()+"-"+new Date().getFullYear();});
  if(!student)return null;
  const existing=(D.invitations||[]).find(i=>i.sid===student.id&&i.status!=="rejected");
  const save=()=>{if(!code.trim())return;up({invitations:[...(D.invitations||[]),{id:uid(),code:code.trim(),sid:student.id,tid:authUid,parId:null,dt:td(),status:"pending"}]});onClose();};
  return <Mdl title={"Invitar familia: "+student.nm} onClose={onClose}><div>
    {existing?<div style={{padding:16,background:existing.status==="accepted"?PL:"#FFFBEB",borderRadius:10,textAlign:"center"}}><div style={{fontSize:13,fontWeight:600,color:existing.status==="accepted"?"#059669":"#D97706",marginBottom:4}}>{existing.status==="accepted"?"Familia vinculada":"Invitación pendiente"}</div><div style={{fontSize:20,fontWeight:800,color:TX,letterSpacing:2,fontFamily:"monospace",padding:"8px 0"}}>{existing.code}</div>{existing.status==="pending"&&<div style={{fontSize:11,color:TM}}>La familia debe usar este código al registrarse</div>}{existing.status==="accepted"&&<div style={{fontSize:11,color:TM}}>Vinculado a: {(D.parents||[]).find(p=>p.id===existing.parId)?.nm||"—"}</div>}</div>:<div>
    <div style={{padding:12,background:PL,borderRadius:10,marginBottom:12,textAlign:"center"}}><div style={{fontSize:11,color:PD,marginBottom:4}}>Código de invitación</div><input value={code} onChange={e=>setCode(e.target.value.toUpperCase())} style={{...inp,textAlign:"center",fontSize:18,fontWeight:800,letterSpacing:3,fontFamily:"monospace",maxWidth:200,margin:"0 auto"}}/></div>
    <div style={{fontSize:12,color:TM,marginBottom:10,padding:"8px 10px",background:BL,borderRadius:8}}>La familia de {student.nm} deberá introducir este código al registrarse para vincular su cuenta con el perfil de su hijo/a.</div>
    <button onClick={save} style={{...bp(),width:"100%"}}>Generar invitación</button></div>}
  </div></Mdl>;
}
/* New: Guided Evaluation Modal - Activity-based observation */
function GuidedEvalModal({student,D,up,onClose}){
  const [phase,setPhase]=useState("pick");
  const [selAct,setSelAct]=useState(null);
  const [scores,setScores]=useState({});
  const [notes,setNotes]=useState("");
  const [gSearch,setGSearch]=useState("");
  const [gDomF,setGDomF]=useState("all");
  if(!student)return null;
  const lastEv=(D.evs||[]).filter(e=>e.sid===student.id).sort((a,b)=>b.dt.localeCompare(a.dt))[0];
  const pctl=getPctl(student.dob);
  const weakDoms=lastEv?DOM.filter(d=>(lastEv.sc[d.k]||0)<pctl.P50).map(d=>d.k):DOM.map(d=>d.k);
  const allA=allActs(D);
  const recAct=allA.filter(a=>weakDoms.includes(a.dom));
  const restAct=allA.filter(a=>!weakDoms.includes(a.dom));
  const filteredAll=(()=>{let r=gDomF==="all"?allA:allA.filter(a=>a.dom===gDomF);if(gSearch)r=r.filter(a=>(a.t+a.desc+a.obj+(a.sub||"")+(a.tags||[]).join(" ")).toLowerCase().includes(gSearch.toLowerCase()));return r;})();
  const actDom=selAct?DOM.find(d=>d.k===selAct.dom):null;
  const domItems=actDom?ITEMS[actDom.k]:[];
  const OBS_SCALE=[{v:0,l:"No observado",c:"#94A3B8"},{v:25,l:"Emergente",c:"#DC2626"},{v:50,l:"En desarrollo",c:"#D97706"},{v:75,l:"Adecuado",c:"#0D9488"},{v:100,l:"Excelente",c:"#059669"}];
  const calcResult=()=>{if(!actDom)return 0;const vals=domItems.map((_,i)=>scores[actDom.k+"_"+i]||0);return vals.length?Math.round(vals.reduce((a,b)=>a+b,0)/vals.length):0;};
  const saveEval=()=>{if(!actDom)return;const domScore=calcResult();const prevSc=lastEv?{...lastEv.sc}:{};DOM.forEach(d=>{if(!prevSc[d.k])prevSc[d.k]=50;});prevSc[actDom.k]=domScore;const newAls=[...(D.als||[])];const dp=getDomPctl(actDom.k,student.dob);if(domScore<dp.P25&&!newAls.find(a=>a.sid===student.id&&a.dom===actDom.k&&a.st==="active")){newAls.push({id:uid(),sid:student.id,dom:actDom.k,msg:actDom.l+(domScore<dp.P10?" bajo P10 — Derivación":" bajo P25"),st:"active",sev:domScore<dp.P10?"high":"medium"});}up({evs:[...(D.evs||[]),{id:uid(),sid:student.id,dt:td(),sc:prevSc}],als:newAls,actAssign:[...(D.actAssign||[]),{id:uid(),sid:student.id,act:selAct.t,dom:selAct.dom,dt:td(),done:true,notes:notes||"Evaluación guiada"}],guidedEvals:[...(D.guidedEvals||[]),{id:uid(),sid:student.id,act:selAct.t,dom:actDom.k,dt:td(),scores:{...scores},result:domScore}]});onClose();};
  const nxtAct=selAct?nextLvlAct(selAct,D):null;
  return <Mdl title={"Evaluación guiada: "+student.nm} onClose={onClose} wide><div>
    {phase==="pick"&&<div>
      <div style={{fontSize:13,color:TX,marginBottom:6,fontWeight:600}}>Elige una actividad de observación</div>
      <div style={{fontSize:12,color:TM,marginBottom:8}}>Realiza la actividad con {student.nm} y después evalúa lo observado. Los resultados se calcularán automáticamente.</div>
      <input value={gSearch} onChange={e=>setGSearch(e.target.value)} placeholder="Buscar actividad..." style={{...inp,marginBottom:6,fontSize:12}}/>
      <div style={{display:"flex",gap:3,marginBottom:8,flexWrap:"wrap"}}><button onClick={()=>setGDomF("all")} style={bo(gDomF==="all",true)}>Todas</button>{DOM.map(d=><button key={d.k} onClick={()=>setGDomF(d.k)} style={{...bo(gDomF===d.k,true),color:gDomF===d.k?"#fff":d.c,background:gDomF===d.k?d.c:d.c+"12",borderColor:d.c,fontSize:10}}>{d.l}</button>)}</div>
      {!gSearch&&gDomF==="all"&&recAct.length>0&&<div style={{marginBottom:10}}><div style={{fontSize:11,fontWeight:600,color:PD,marginBottom:4}}>Recomendadas para {student.nm.split(" ")[0]} (áreas a reforzar)</div>{recAct.slice(0,6).map((a,i)=>{const d=DOM.find(x=>x.k===a.dom);return <div key={i} onClick={()=>setSelAct(a)} style={{display:"flex",alignItems:"center",gap:8,padding:8,borderRadius:8,cursor:"pointer",background:selAct?.t===a.t?PL:"transparent",border:selAct?.t===a.t?"1.5px solid "+P:"1.5px solid "+BL,marginBottom:3}}><div style={{width:28,height:28,borderRadius:8,background:d?.c+"15",display:"flex",alignItems:"center",justifyContent:"center"}}>{d?.I&&<d.I size={14} color={d.c}/>}</div><div style={{flex:1}}><div style={{fontSize:12,fontWeight:500}}>{a.t}{a.lvl&&<span style={{fontSize:9,color:TM,marginLeft:4}}>Nv.{a.lvl}</span>}</div><div style={{fontSize:10,color:TM}}>{d?.l} · {a.obj} · {a.dur}{a.sub?(" · "+a.sub):""}</div></div><Pill t={a.dif} co={TM} bg={BL}/></div>})}</div>}
      <div style={{marginBottom:6,maxHeight:240,overflowY:"auto"}}><div style={{fontSize:11,fontWeight:600,color:TM,marginBottom:4}}>{gSearch||gDomF!=="all"?"Resultados ("+filteredAll.length+")":"Todas las actividades"}</div>{(gSearch||gDomF!=="all"?filteredAll:restAct).slice(0,12).map((a,i)=>{const d=DOM.find(x=>x.k===a.dom);return <div key={i} onClick={()=>setSelAct(a)} style={{display:"flex",alignItems:"center",gap:8,padding:6,borderRadius:8,cursor:"pointer",background:selAct?.t===a.t?PL:"transparent",border:selAct?.t===a.t?"1.5px solid "+P:"1.5px solid transparent",marginBottom:2}}><div style={{width:24,height:24,borderRadius:6,background:d?.c+"15",display:"flex",alignItems:"center",justifyContent:"center"}}>{d?.I&&<d.I size={12} color={d.c}/>}</div><div style={{flex:1}}><div style={{fontSize:11,fontWeight:500}}>{a.t}{a.lvl&&<span style={{fontSize:9,color:TM,marginLeft:3}}>Nv.{a.lvl}</span>}</div><div style={{fontSize:10,color:TM}}>{d?.l} · {a.dur}{a.sub?(" · "+a.sub):""}</div></div></div>})}</div>
      <button onClick={()=>{if(selAct)setPhase("observe");}} disabled={!selAct} style={{...bp(),width:"100%",marginTop:6,opacity:selAct?1:.5}}>Comenzar observación</button>
    </div>}
    {phase==="observe"&&selAct&&actDom&&<div>
      <div style={{padding:10,background:actDom.c+"10",borderRadius:10,marginBottom:12,border:"1px solid "+actDom.c+"30"}}><div style={{display:"flex",alignItems:"center",gap:6,marginBottom:4}}><actDom.I size={16} color={actDom.c}/><span style={{fontSize:14,fontWeight:600,color:actDom.c}}>{selAct.t}</span></div><div style={{fontSize:12,color:TM}}>{selAct.desc}</div>{selAct.mats&&<div style={{fontSize:11,color:TM,marginTop:4}}>Materiales: {selAct.mats}</div>}<div style={{display:"flex",gap:3,marginTop:6}}>{OBS_SCALE.map(s=><div key={s.v} style={{flex:1,textAlign:"center",padding:"2px 0",borderRadius:4,background:s.c+"12",border:"1px solid "+s.c+"30"}}><div style={{fontSize:8,fontWeight:600,color:s.c}}>{s.l}</div><div style={{fontSize:8,color:TM}}>{s.v}pts</div></div>)}</div></div>
      <div style={{display:"flex",justifyContent:"space-between",alignItems:"center",marginBottom:8}}><div style={{fontSize:13,fontWeight:600,color:TX}}>Evalúa lo observado en {actDom.l}</div><div style={{fontSize:11,color:TM}}>{domItems.filter((_,i)=>scores[actDom.k+"_"+i]!==undefined).length}/{domItems.length} ítems</div></div>
      {domItems.map((item,i)=><div key={i} style={{marginBottom:10,padding:10,background:scores[actDom.k+"_"+i]!==undefined?BG:"#fff",borderRadius:8,border:scores[actDom.k+"_"+i]!==undefined?"1.5px solid "+BD:"1.5px solid #E2E8F0"}}><div style={{fontSize:12,fontWeight:600,marginBottom:5,color:TX}}>{i+1}. {item}</div><div style={{display:"flex",gap:3}}>{OBS_SCALE.map(s=><button key={s.v} onClick={()=>setScores({...scores,[actDom.k+"_"+i]:s.v})} style={{flex:1,padding:"6px 2px",borderRadius:6,border:scores[actDom.k+"_"+i]===s.v?"2px solid "+s.c:"2px solid "+BD,background:scores[actDom.k+"_"+i]===s.v?s.c+"15":"#fff",color:scores[actDom.k+"_"+i]===s.v?s.c:TM,fontSize:9,cursor:"pointer",fontFamily:F,fontWeight:scores[actDom.k+"_"+i]===s.v?700:400,textAlign:"center"}}>{s.l}</button>)}</div></div>)}
      <textarea value={notes} onChange={e=>setNotes(e.target.value)} placeholder="Notas adicionales sobre la observación..." rows={2} style={{...inp,marginBottom:8,resize:"vertical"}}/>
      <div style={{padding:12,background:actDom.c+"10",borderRadius:10,marginBottom:8,border:"1px solid "+actDom.c+"30"}}><div style={{display:"flex",justifyContent:"space-between",alignItems:"center"}}><div><div style={{fontSize:11,color:TM}}>Resultado en {actDom.l}</div><div style={{fontSize:32,fontWeight:800,color:actDom.c}}>{calcResult()}%</div><div style={{fontSize:11,color:percCo(calcResult()),fontWeight:600}}>{percLb(calcResult())}</div></div><div style={{textAlign:"right"}}>{(()=>{const dp=getDomPctl(actDom.k,student.dob);const r=calcResult();const sev2=r<dp.P10?"danger":r<dp.P25?"warning":r>dp.P90?"excellent":"normal";return <div><div style={{fontSize:10,color:TM,marginBottom:2}}>Comparado con su edad</div>{r<dp.P10&&<div style={{fontSize:11,fontWeight:700,color:"#DC2626",padding:"4px 8px",background:"#FEF2F2",borderRadius:6}}>⚠ Bajo P10 — Derivación</div>}{r>=dp.P10&&r<dp.P25&&<div style={{fontSize:11,fontWeight:700,color:"#D97706",padding:"4px 8px",background:"#FFFBEB",borderRadius:6}}>👀 P10-P25 — Seguimiento</div>}{r>=dp.P25&&r<=dp.P90&&<div style={{fontSize:11,fontWeight:700,color:P,padding:"4px 8px",background:PL,borderRadius:6}}>✓ Dentro del rango esperado</div>}{r>dp.P90&&<div style={{fontSize:11,fontWeight:700,color:"#059669",padding:"4px 8px",background:"#ECFDF5",borderRadius:6}}>🌟 Excelente — por encima P90</div>}<div style={{fontSize:9,color:TM,marginTop:3}}>P10:{dp.P10} · P25:{dp.P25} · P50:{dp.P50} · P90:{dp.P90}</div></div>})()}</div></div>{lastEv&&<div style={{marginTop:6,padding:6,background:"#fff",borderRadius:6}}><div style={{fontSize:10,color:TM}}>Resultado anterior en {actDom.l}: <b style={{color:percCo(lastEv.sc[actDom.k]||0)}}>{lastEv.sc[actDom.k]||0}%</b> → <b style={{color:percCo(calcResult())}}>{calcResult()}%</b> <span style={{color:calcResult()>=(lastEv.sc[actDom.k]||0)?"#059669":"#DC2626"}}>{calcResult()>=(lastEv.sc[actDom.k]||0)?"+":"−"}{Math.abs(calcResult()-(lastEv.sc[actDom.k]||0))}pts</span></div></div>}</div>
      <div style={{display:"flex",gap:6}}><button onClick={()=>setPhase("pick")} style={{...bo(false),flex:1}}>Atrás</button><button onClick={saveEval} style={{...bp(),flex:1}}>Guardar evaluación</button></div>
      {nxtAct&&<div style={{marginTop:8,padding:8,background:"#F0F9FF",borderRadius:8,border:"1px solid #BAE6FD"}}><div style={{fontSize:10,fontWeight:600,color:"#0369A1"}}>Siguiente nivel disponible →</div><div style={{display:"flex",alignItems:"center",gap:6,marginTop:3}}><div style={{width:20,height:20,borderRadius:99,background:"#0369A1"+"20",display:"flex",alignItems:"center",justifyContent:"center",fontSize:9,fontWeight:700,color:"#0369A1"}}>{nxtAct.lvl||2}</div><div><div style={{fontSize:11,fontWeight:500}}>{nxtAct.t}</div><div style={{fontSize:9,color:TM}}>{nxtAct.obj} · {nxtAct.dif} · {nxtAct.dur}</div></div></div></div>}
    </div>}
  </div></Mdl>;
}
function CreateActModal({D,up,onClose}){
  const [dom,setDom2]=useState("cog");const [t,setT]=useState("");const [desc,setDesc]=useState("");const [dur2,setDur2]=useState("15 min");const [age,setAge]=useState("3-6");const [dif,setDif]=useState("Media");const [obj,setObj]=useState("");const [mats,setMats]=useState("");const [lvl,setLvl]=useState(1);const [sub,setSub]=useState("");const [tags2,setTags2]=useState("");
  const save=()=>{if(!t.trim()||!desc.trim())return;const newAct={dom,t:t.trim(),desc:desc.trim(),dur:dur2,age,dif,obj:obj.trim()||t.trim(),mats:mats.trim(),lvl,sub:sub||ACT_SUBS[dom]?.[0]||"",tags:tags2.split(",").map(x=>x.trim()).filter(Boolean),custom:true};up({customActs:[...(D.customActs||[]),newAct]});onClose();};
  const curDom=DOM.find(d=>d.k===dom);
  return <Mdl title="Crear actividad personalizada" onClose={onClose} wide><div>
    <div style={{display:"flex",gap:3,marginBottom:10}}>{DOM.map(d=>{const Ic=d.I;return <button key={d.k} onClick={()=>{setDom2(d.k);setSub(ACT_SUBS[d.k]?.[0]||"");}} style={{...bo(dom===d.k,true),flex:1,display:"flex",flexDirection:"column",alignItems:"center",gap:1,color:dom===d.k?"#fff":d.c,background:dom===d.k?d.c:d.c+"12",borderColor:d.c}}><Ic size={12}/><span style={{fontSize:8}}>{d.l}</span></button>})}</div>
    <input value={t} onChange={e=>setT(e.target.value)} placeholder="Nombre de la actividad" style={{...inp,marginBottom:6,fontWeight:600}}/>
    <textarea value={desc} onChange={e=>setDesc(e.target.value)} placeholder="Descripción detallada..." rows={2} style={{...inp,marginBottom:6,resize:"vertical"}}/>
    <div style={{display:"grid",gridTemplateColumns:"1fr 1fr",gap:6,marginBottom:6}}>
      <div><label style={{fontSize:10,color:TM}}>Objetivo</label><input value={obj} onChange={e=>setObj(e.target.value)} placeholder="Ej: Memoria de trabajo" style={inp}/></div>
      <div><label style={{fontSize:10,color:TM}}>Materiales</label><input value={mats} onChange={e=>setMats(e.target.value)} placeholder="Ej: Tarjetas, pelotas" style={inp}/></div>
    </div>
    <div style={{display:"grid",gridTemplateColumns:"1fr 1fr 1fr",gap:6,marginBottom:6}}>
      <div><label style={{fontSize:10,color:TM}}>Duración</label><select value={dur2} onChange={e=>setDur2(e.target.value)} style={inp}><option>5 min</option><option>10 min</option><option>15 min</option><option>20 min</option><option>30 min</option></select></div>
      <div><label style={{fontSize:10,color:TM}}>Edad</label><select value={age} onChange={e=>setAge(e.target.value)} style={inp}><option>3-4</option><option>3-5</option><option>3-6</option><option>4-5</option><option>4-6</option><option>5-6</option></select></div>
      <div><label style={{fontSize:10,color:TM}}>Dificultad</label><select value={dif} onChange={e=>setDif(e.target.value)} style={inp}><option>Fácil</option><option>Media</option><option>Alta</option></select></div>
    </div>
    <div style={{display:"grid",gridTemplateColumns:"1fr 1fr",gap:6,marginBottom:6}}>
      <div><label style={{fontSize:10,color:TM}}>Nivel de progresión</label><div style={{display:"flex",gap:3}}>{[1,2,3].map(l=><button key={l} onClick={()=>setLvl(l)} style={{...bo(lvl===l,true),flex:1,color:lvl===l?"#fff":[P,"#D97706","#DC2626"][l-1],background:lvl===l?[P,"#F59E0B","#DC2626"][l-1]:"#fff",borderColor:[P,"#F59E0B","#DC2626"][l-1]}}>Nv.{l}</button>)}</div></div>
      <div><label style={{fontSize:10,color:TM}}>Sub-objetivo</label><select value={sub} onChange={e=>setSub(e.target.value)} style={inp}>{(ACT_SUBS[dom]||[]).map(s=><option key={s}>{s}</option>)}</select></div>
    </div>
    <div><label style={{fontSize:10,color:TM}}>Tags (separar con comas)</label><input value={tags2} onChange={e=>setTags2(e.target.value)} placeholder="Ej: memoria, visual, cooperación" style={{...inp,marginBottom:8}}/></div>
    {t.trim()&&<div style={{padding:10,background:curDom?.c+"08",borderRadius:8,marginBottom:8,border:"1px solid "+curDom?.c+"30"}}><div style={{fontSize:11,fontWeight:600,color:TX}}>Vista previa</div><div style={{display:"flex",alignItems:"center",gap:4,marginTop:4}}>{curDom?.I&&<curDom.I size={13} color={curDom.c}/>}<span style={{fontSize:12,fontWeight:600}}>{t}</span></div><div style={{fontSize:10,color:TM,marginTop:2}}>{desc.slice(0,80)}</div><div style={{display:"flex",gap:2,marginTop:4}}><Pill t={"Nv."+lvl} co={lvl===1?P:lvl===2?"#D97706":"#DC2626"} bg={lvl===1?PL:lvl===2?"#FFFBEB":"#FEF2F2"}/><Pill t={dur2} co={TM} bg={BL}/><Pill t={sub||"—"} co={curDom?.c} bg={curDom?.c+"15"}/></div></div>}
    <button onClick={save} disabled={!t.trim()||!desc.trim()} style={{...bp(),width:"100%",opacity:t.trim()&&desc.trim()?1:.5}}>Crear actividad</button>
  </div></Mdl>;
}
/* New: Daily Analysis Modal */
function DailyAnModal({D,up,sts,onClose}){
  const [focus,setFocus]=useState("");const [summ,setSumm]=useState("");const [hl,setHl]=useState("");const [cn,setCn]=useState("");
  const save=()=>{if(!summ.trim())return;up({dailyAn:[{dt:td(),focus:focus||"General",summary:summ.trim(),highlights:hl.split("\n").filter(x=>x.trim()),concerns:cn.split("\n").filter(x=>x.trim()),domFocus:{}},...(D.dailyAn||[])]});onClose();};
  return <Mdl title="Nuevo análisis diario" onClose={onClose} wide><div>
    <label style={{fontSize:12,color:TM}}>Foco del día</label>
    <input value={focus} onChange={e=>setFocus(e.target.value)} placeholder="Ej: Emocional y Social" style={{...inp,marginBottom:8}}/>
    <label style={{fontSize:12,color:TM}}>Resumen</label>
    <textarea value={summ} onChange={e=>setSumm(e.target.value)} placeholder="Resumen del análisis..." rows={3} style={{...inp,marginBottom:8,resize:"vertical"}}/>
    <label style={{fontSize:12,color:TM}}>Logros destacados (uno por línea)</label>
    <textarea value={hl} onChange={e=>setHl(e.target.value)} placeholder="Martín jugó en grupo 15 min..." rows={2} style={{...inp,marginBottom:8,resize:"vertical"}}/>
    <label style={{fontSize:12,color:TM}}>Preocupaciones (uno por línea)</label>
    <textarea value={cn} onChange={e=>setCn(e.target.value)} placeholder="Daniel distracción en asamblea..." rows={2} style={{...inp,marginBottom:8,resize:"vertical"}}/>
    <button onClick={save} disabled={!summ.trim()} style={{...bp(),width:"100%",opacity:summ.trim()?1:.5}}>Guardar análisis</button>
  </div></Mdl>;
}

function SNav({items,active,onNav,sub,userName,onLogout}){
  const [open,setOpen]=useState(false);
  const toggle=()=>setOpen(!open);
  const go=(k)=>{onNav(k);setOpen(false);};
  const totalBadge=items.reduce((a,i)=>a+(i.badge||0),0);
  return <>
    {/* Collapsed strip — always visible */}
    <div style={{width:52,background:"#fff",borderRight:"1px solid "+BD,display:"flex",flexDirection:"column",height:"100vh",position:"sticky",top:0,flexShrink:0,zIndex:90}}>
      <button onClick={toggle} style={{display:"flex",alignItems:"center",justifyContent:"center",padding:"16px 0",border:"none",background:"transparent",cursor:"pointer",color:PD,position:"relative"}}><Menu size={20}/>{totalBadge>0&&!open&&<span style={{position:"absolute",top:10,right:8,width:8,height:8,borderRadius:99,background:"#EF4444"}}/>}</button>
      <div style={{padding:"8px 0",borderTop:"1px solid "+BL,borderBottom:"1px solid "+BL}}><div style={{display:"flex",justifyContent:"center",padding:"4px 0"}}><NLogo sz={28}/></div></div>
      <div style={{flex:1,overflowY:"auto",padding:"6px 0"}}>{items.map(it=>{const Ic=it.I;return <button key={it.k} onClick={()=>go(it.k)} title={it.l} style={{display:"flex",alignItems:"center",justifyContent:"center",width:"100%",padding:"10px 0",border:"none",background:active===it.k?PL:"transparent",color:active===it.k?PD:TM,cursor:"pointer",position:"relative",borderLeft:active===it.k?"3px solid "+P:"3px solid transparent"}}><Ic size={16}/>{it.badge>0&&<span style={{position:"absolute",top:4,right:6,background:"#EF4444",color:"#fff",fontSize:7,fontWeight:700,padding:"0 3px",borderRadius:99,minWidth:12,textAlign:"center"}}>{it.badge}</span>}</button>})}</div>
      <div style={{padding:"10px 0",borderTop:"1px solid "+BD,display:"flex",justifyContent:"center"}}><Av nm={userName} sz={28}/></div>
    </div>
    {/* Overlay */}
    {open&&<div onClick={()=>setOpen(false)} style={{position:"fixed",inset:0,background:"rgba(15,23,42,.3)",zIndex:98}}/>}
    {/* Expanded sidebar panel */}
    <div style={{position:"fixed",top:0,left:0,width:280,height:"100vh",background:"#fff",boxShadow:open?"4px 0 24px rgba(0,0,0,.12)":"none",transform:open?"translateX(0)":"translateX(-110%)",transition:"transform .25s cubic-bezier(.4,0,.2,1)",zIndex:99,display:"flex",flexDirection:"column",overflowY:"auto"}}>
      <div style={{display:"flex",alignItems:"center",justifyContent:"space-between",padding:"16px 18px",borderBottom:"1px solid "+BD}}>
        <div style={{display:"flex",alignItems:"center",gap:10}}><NLogo sz={32}/><div><div style={{fontWeight:800,fontSize:15,fontFamily:FD}}>NEXUS KIDS</div><div style={{fontSize:11,color:TM}}>{sub}</div></div></div>
        <button onClick={()=>setOpen(false)} style={{width:32,height:32,borderRadius:8,border:"none",background:BL,cursor:"pointer",color:TM,display:"flex",alignItems:"center",justifyContent:"center"}}><X size={16}/></button>
      </div>
      <div style={{flex:1,padding:"12px 0"}}>
        <div style={{padding:"0 14px 8px",fontSize:9,fontWeight:700,color:TL,textTransform:"uppercase",letterSpacing:1}}>{"Navegación"}</div>
        {items.map(it=>{const Ic=it.I;const isActive=active===it.k;return <button key={it.k} onClick={()=>go(it.k)} style={{display:"flex",alignItems:"center",gap:10,width:"100%",padding:"11px 18px",border:"none",background:isActive?"linear-gradient(90deg,"+PL+",#fff)":"transparent",color:isActive?PD:TX,cursor:"pointer",fontFamily:F,fontSize:13,fontWeight:isActive?700:400,textAlign:"left",borderLeft:isActive?"4px solid "+P:"4px solid transparent",position:"relative",transition:"background .15s"}}><div style={{width:30,height:30,borderRadius:8,background:isActive?P+"18":BL,display:"flex",alignItems:"center",justifyContent:"center",flexShrink:0}}><Ic size={15} color={isActive?PD:TM}/></div>{it.l}{it.badge>0&&<span style={{marginLeft:"auto",background:"#EF4444",color:"#fff",fontSize:10,fontWeight:700,padding:"2px 7px",borderRadius:99}}>{it.badge}</span>}</button>})}
      </div>
      <div style={{padding:"14px 18px",borderTop:"1px solid "+BD,background:BG}}>
        <div style={{display:"flex",alignItems:"center",gap:10,marginBottom:10}}><Av nm={userName} sz={36}/><div><div style={{fontSize:13,fontWeight:700}}>{userName}</div><div style={{fontSize:10,color:TM}}>{sub}</div></div></div>
        <button onClick={()=>{setOpen(false);onLogout();}} style={{display:"flex",alignItems:"center",justifyContent:"center",gap:6,width:"100%",padding:"8px 10px",border:"none",background:"#fff",borderRadius:8,cursor:"pointer",color:TM,fontSize:12,fontFamily:F,fontWeight:500,boxShadow:"0 1px 3px rgba(0,0,0,.08)"}}><LogOut size={13}/>{"Cerrar sesión"}</button>
              </div>
    </div>
  </>;
}

/* ═══════════ TEACHER APP ═══════════ */
function TeacherApp({D,up,logout}){
  const auth=D.auth||{};
  const [v,setV]=useState("dash");
  const [sel,setSel]=useState(null);
  const [mdl,setMdl]=useState(null);
  const [actF,setActF]=useState("all");
  const [analTab,setAnalTab]=useState("pctl");
  const [stTab,setStTab]=useState("overview");
  const [repTab,setRepTab]=useState("individual");
  const [pdfSid,setPdfSid]=useState(null);
  const user=(D.users||[]).find(u=>u.id===auth.uid)||{nm:"Docente"};
  const myCls=(D.cls||[]).filter(c=>c.tid===auth.uid);
  const ac=myCls[0]?.id||"";
  const sts=(D.sts||[]).filter(s=>s.cid===ac);
  const le=sid=>{const e=(D.evs||[]).filter(e2=>e2.sid===sid);return e.length?e.sort((a,b)=>b.dt.localeCompare(a.dt))[0]:null;};
  const allEvs=sid=>(D.evs||[]).filter(e=>e.sid===sid).sort((a,b)=>a.dt.localeCompare(b.dt));
  const aAlerts=(D.als||[]).filter(a=>a.st==="active"&&sts.find(s=>s.id===a.sid));
  const navs=[{k:"dash",l:"Dashboard",I:BarChart3},{k:"sts",l:"Alumnos",I:Users},{k:"eval",l:"Evaluar",I:Clipboard},{k:"analysis",l:"Análisis",I:PieChart},{k:"acts",l:"Actividades",I:Play},{k:"journal",l:"Diario",I:BookOpen},{k:"alerts",l:"Alertas",I:Bell,badge:aAlerts.length},{k:"track",l:"Seguimiento",I:Target},{k:"reports",l:"Informes",I:FileText},{k:"msgs",l:"Familias",I:MessageSquare}];

  const content=()=>{
    /* DASHBOARD */
    if(v==="dash"){
      if(!myCls.length)return <div style={{...cd(),padding:40,textAlign:"center"}}><School size={40} color={BD}/><h3 style={{fontFamily:FD,fontSize:16,margin:"12px 0 6px"}}>Bienvenido, {(user.nm||"").split(" ")[0]}</h3><p style={{fontSize:13,color:TM}}>Crea tu primera aula</p><button onClick={()=>setMdl("addCls")} style={bp()}>+ Crear aula</button></div>;
      const avg={};let has=false;DOM.forEach(d=>{const vals=sts.map(s=>le(s.id)).filter(Boolean).map(e=>e.sc[d.k]||0);if(vals.length){avg[d.k]=Math.round(vals.reduce((a,b)=>a+b,0)/vals.length);has=true;}else avg[d.k]=0;});
      const evCount=(D.evs||[]).filter(e=>sts.some(s=>s.id===e.sid)).length;
      const latestDA=(D.dailyAn||[])[0];
      const actDone=(D.actAssign||[]).filter(a=>sts.some(s=>s.id===a.sid)&&a.done).length;
      const actTotal=(D.actAssign||[]).filter(a=>sts.some(s=>s.id===a.sid)).length;
      return <div>
        <h2 style={{fontFamily:FD,fontSize:20,margin:"0 0 4px"}}>Buenos días, {(user.nm||"").split(" ")[0]}</h2>
        <p style={{fontSize:12,color:TM,margin:"0 0 14px"}}>{D.school?.name||""} · {myCls[0]?.name||""} · {myCls[0]?.grade||""} · {new Date().toLocaleDateString("es-ES",{weekday:"long",day:"numeric",month:"long"})}</p>
        <div style={{display:"grid",gridTemplateColumns:"repeat(4,1fr)",gap:10,marginBottom:14}}>{[{l:"Alumnos",v:sts.length,bg:PL,I:Users},{l:"Alertas",v:aAlerts.length,bg:aAlerts.length?"#FEF2F2":PL,I:Bell},{l:"Evals",v:evCount,bg:"#F0F9FF",I:Clipboard},{l:"Actividades",v:actDone+"/"+actTotal,bg:"#ECFDF5",I:Play}].map((k,i)=>{const Ic2=k.I;return <div key={i} style={{...cd(),padding:12,background:k.bg}}><Ic2 size={16} color={TM} style={{marginBottom:4}}/><div style={{fontFamily:FD,fontSize:k.v.toString().length>4?16:22,color:TX}}>{k.v}</div><div style={{fontSize:10,color:TM}}>{k.l}</div></div>})}</div>
        {latestDA&&<div style={{...cd(),padding:14,marginBottom:14,borderLeft:"4px solid "+P}}><div style={{display:"flex",justifyContent:"space-between",alignItems:"center",marginBottom:6}}><h3 style={{fontFamily:FD,fontSize:14,margin:0}}>Análisis del día — {fmt(latestDA.dt)}</h3><Pill t={latestDA.focus} co={P} bg={PL}/></div><p style={{fontSize:12,color:TM,margin:"0 0 8px"}}>{latestDA.summary}</p>{latestDA.highlights&&<div style={{marginBottom:4}}>{latestDA.highlights.map((h,i)=><div key={i} style={{fontSize:11,color:"#059669",padding:"2px 0"}}><CheckCircle size={10} style={{verticalAlign:"middle",marginRight:3}}/>{h}</div>)}</div>}{latestDA.concerns&&latestDA.concerns.length>0&&<div>{latestDA.concerns.map((c,i)=><div key={i} style={{fontSize:11,color:"#D97706",padding:"2px 0"}}><AlertTriangle size={10} style={{verticalAlign:"middle",marginRight:3}}/>{c}</div>)}</div>}</div>}
        {(()=>{const needsObs=sts.filter(s=>{const ev2=le(s.id);if(!ev2)return true;const pctl2=getPctl(s.dob);return DOM.some(d=>(ev2.sc[d.k]||0)<pctl2.P25);}).slice(0,4);if(!needsObs.length)return null;return <div style={{...cd(),padding:14,marginBottom:14,borderLeft:"4px solid #F59E0B",background:"#FFFBEB"}}><div style={{display:"flex",justifyContent:"space-between",alignItems:"center",marginBottom:8}}><div><h3 style={{fontFamily:FD,fontSize:14,margin:0,color:"#D97706"}}>Observaciones sugeridas hoy</h3><p style={{fontSize:11,color:TM,margin:"2px 0 0"}}>Alumnos que necesitan evaluación guiada por actividades</p></div><Play size={16} color="#F59E0B"/></div>{needsObs.map(s=>{const ev2=le(s.id);const pctl2=getPctl(s.dob);const weakDom=ev2?DOM.filter(d=>(ev2.sc[d.k]||0)<pctl2.P25):DOM.slice(0,2);const sugAct=allActs(D).filter(a=>weakDom.some(d=>d.k===a.dom))[0];return <div key={s.id} onClick={()=>setMdl({t:"guided",s})} style={{display:"flex",alignItems:"center",gap:8,padding:8,background:"#fff",borderRadius:8,marginBottom:4,cursor:"pointer",border:"1px solid #FDE68A"}}><Av nm={s.nm} sz={28}/><div style={{flex:1}}><div style={{fontSize:12,fontWeight:600}}>{s.nm}</div><div style={{fontSize:10,color:TM}}>{ev2?"Áreas: "+weakDom.map(d=>d.l).join(", "):"Sin evaluar — necesita primera evaluación"}</div>{sugAct&&<div style={{fontSize:10,color:"#D97706",marginTop:1}}>Actividad sugerida: {sugAct.t}</div>}</div><ChevronRight size={14} color={TM}/></div>})}</div>})()}
        {has&&<div style={{...cd(),padding:14,marginBottom:14}}><h3 style={{fontFamily:FD,fontSize:14,margin:"0 0 8px"}}>Perfil medio del aula vs P50</h3><Bars sc={avg} pctl={getPctl("2021-06-01")}/></div>}
        {aAlerts.length>0&&<div style={{...cd(),padding:12,marginBottom:14,borderLeft:"4px solid #EF4444"}}><h3 style={{fontFamily:FD,fontSize:13,margin:"0 0 6px",color:"#DC2626"}}>Alertas activas</h3>{aAlerts.slice(0,3).map(a=>{const s2=(D.sts||[]).find(x=>x.id===a.sid);return <div key={a.id} style={{fontSize:12,color:TM,padding:"3px 0"}}><span style={{fontWeight:600,color:TX}}>{s2?.nm}</span> — {a.msg}</div>})}{aAlerts.length>3&&<button onClick={()=>setV("alerts")} style={{fontSize:11,color:P,background:"none",border:"none",cursor:"pointer",fontFamily:F,marginTop:4}}>Ver todas ({aAlerts.length})</button>}</div>}
        <div style={{...cd(),padding:14}}><div style={{display:"flex",justifyContent:"space-between",marginBottom:8}}><h3 style={{fontFamily:FD,fontSize:14,margin:0}}>{"Alumnos"}</h3><div style={{display:"flex",gap:4}}><button onClick={()=>setMdl("addSt")} style={bp(true)}>{"+ "+"Alumnos"}</button><button onClick={()=>setMdl("addCls")} style={bo(false,true)}>+ Aula</button></div></div>
        {sts.length===0?<p style={{color:TL,textAlign:"center"}}>Aula vacía</p>:sts.map(s=>{const ev=le(s.id);const g2=ev?gs(ev.sc):null;const pctl=getPctl(s.dob);const sev=g2!==null?sevLb(g2,pctl):null;return <div key={s.id} onClick={()=>{setSel(s.id);setV("sts");}} style={{display:"flex",alignItems:"center",gap:10,padding:"8px 0",borderBottom:"1px solid "+BL,cursor:"pointer"}}><Av nm={s.nm} sz={32}/><div style={{flex:1}}><div style={{fontSize:13,fontWeight:600}}>{s.nm}</div><div style={{fontSize:11,color:TL}}>{ageFn(s.dob)}</div></div>{g2!==null&&<Pill t={g2+"% · "+percLb(g2)} co={percCo(g2)} bg={percBg(g2)}/>}</div>})}</div>
      </div>;
    }
    /* STUDENT DETAIL */
    if(v==="sts"&&sel){
      const s=(D.sts||[]).find(x=>x.id===sel);if(!s)return <p>No encontrado</p>;
      const ev=le(s.id);const pctl=getPctl(s.dob);const sAls=(D.als||[]).filter(a=>a.sid===s.id&&a.st==="active");const sObs=(D.obs||[]).filter(o=>o.sid===s.id);const sMil=(D.milestones||[]).filter(m=>m.sid===s.id);const sPmi=(D.pmi||[]).filter(p=>p.sid===s.id);const sEvs=allEvs(s.id);const sPn=(D.pNotes||[]).filter(p=>p.sid===s.id);const sAA=(D.actAssign||[]).filter(a=>a.sid===s.id);const sInc=(D.incidents||[]).filter(i=>i.sid===s.id);
      const prevEv=sEvs.length>=2?sEvs[sEvs.length-2]:null;
      const recA=ev?recActs(ev.sc,pctl,D):[];
      const tabs=[{k:"overview",l:"Resumen"},{k:"domains",l:"Dominios"},{k:"timeline",l:"Timeline"},{k:"activities",l:"Actividades"},{k:"pmi",l:"PMI"},{k:"incidents",l:"Incidencias"}];
      return <div>
        <button onClick={()=>{setSel(null);setStTab("overview");}} style={{background:"none",border:"none",color:P,cursor:"pointer",fontFamily:F,fontSize:13,marginBottom:10}}><ChevronLeft size={14} style={{verticalAlign:"middle"}}/> Volver</button>
        <div style={{...cd(),padding:14,marginBottom:12}}><div style={{display:"flex",alignItems:"center",gap:10}}><Av nm={s.nm} sz={44}/><div style={{flex:1}}><h2 style={{fontFamily:FD,fontSize:17,margin:0}}>{s.nm}</h2><p style={{fontSize:12,color:TM,margin:0}}>{ageFn(s.dob)} · {myCls[0]?.name||""} · Nac: {s.dob}</p></div><div style={{display:"flex",gap:4,flexWrap:"wrap"}}><button onClick={()=>setMdl({t:"eval",s})} style={bp(true)}>{"Evaluar"}</button><button onClick={()=>setMdl({t:"obs",s})} style={bo(false,true)}>{"Observar"}</button></div></div>
          <div style={{display:"flex",gap:3,marginTop:8,flexWrap:"wrap"}}><button onClick={()=>setMdl({t:"guided",s})} style={{...bo(false,true),background:"#FFF7ED",borderColor:"#F59E0B",color:"#D97706"}}><Play size={10}/> Eval. guiada</button><button onClick={()=>setMdl({t:"invite",s})} style={{...bo(false,true),background:"#EFF6FF",borderColor:"#3B82F6",color:"#2563EB"}}><UserPlus size={10}/> Invitar familia</button><button onClick={()=>setMdl({t:"pnote",s})} style={bo(false,true)}><Send size={10}/> Familia</button><button onClick={()=>setMdl({t:"assign",s})} style={bo(false,true)}><Play size={10}/> Actividad</button><button onClick={()=>setMdl({t:"pmi",s})} style={bo(false,true)}><Target size={10}/> PMI</button><button onClick={()=>setMdl({t:"mile",s})} style={bo(false,true)}><Award size={10}/> Hito</button><button onClick={()=>setMdl({t:"inc",s})} style={bo(false,true)}><AlertTriangle size={10}/> Incidencia</button></div>
        </div>
        {sAls.length>0&&<div style={{...cd(),padding:10,marginBottom:12,borderLeft:"4px solid #EF4444"}}><div style={{fontSize:12,fontWeight:600,color:"#DC2626",marginBottom:4}}>Alertas activas ({sAls.length})</div>{sAls.map(al=>{const d=DOM.find(x=>x.k===al.dom);return <div key={al.id} style={{display:"flex",justifyContent:"space-between",alignItems:"center",padding:"4px 0"}}><div style={{fontSize:12,color:TM}}><span style={{color:d?.c,fontWeight:600}}>{d?.l}</span> — {al.msg}</div><button onClick={()=>up({als:(D.als||[]).map(x=>x.id===al.id?{...x,st:"resolved"}:x)})} style={bo(false,true)}>Resolver</button></div>})}</div>}
        {/* Tabs */}
        <div style={{display:"flex",gap:3,marginBottom:12,flexWrap:"wrap"}}>{tabs.map(t=><button key={t.k} onClick={()=>setStTab(t.k)} style={bo(stTab===t.k,true)}>{t.l}</button>)}</div>
        {/* TAB: Overview */}
        {stTab==="overview"&&<div>
          {ev?<div style={{...cd(),padding:14,marginBottom:12}}>
            <div style={{display:"flex",justifyContent:"space-between",alignItems:"center",marginBottom:10}}><h3 style={{fontFamily:FD,fontSize:14,margin:0}}>Score global</h3><div style={{textAlign:"right"}}><div style={{fontSize:18,fontWeight:800,color:percCo(gs(ev.sc)),fontFamily:FD}}>{gs(ev.sc)}%</div><div style={{fontSize:10,color:TM}}>{percLb(gs(ev.sc))} · {pctlLabel(gs(ev.sc),pctl)}</div></div></div>
            <Bars sc={ev.sc} pctl={pctl}/>
            <div style={{marginTop:8,fontSize:11,color:TL}}>Última evaluación: {fmt(ev.dt)} · {sEvs.length} evaluaciones totales</div>
          </div>:<div style={{...cd(),padding:30,textAlign:"center"}}><p style={{color:TL}}>Sin evaluar</p><button onClick={()=>setMdl({t:"eval",s})} style={bp()}>Evaluar ahora</button></div>}
          {sEvs.length>1&&<div style={{...cd(),padding:14,marginBottom:12}}><h3 style={{fontFamily:FD,fontSize:13,margin:"0 0 8px"}}>Evolución ({sEvs.length} evaluaciones)</h3>{sEvs.map((e,i)=>{const g2=gs(e.sc);const prev=i>0?gs(sEvs[i-1].sc):null;const diff=prev!==null?g2-prev:null;return <div key={e.id} style={{display:"flex",justifyContent:"space-between",alignItems:"center",padding:"5px 0",borderBottom:i<sEvs.length-1?"1px solid "+BL:"none"}}><span style={{fontSize:12,color:TM}}>{fmt(e.dt)}</span><div style={{display:"flex",alignItems:"center",gap:8}}>{diff!==null&&<span style={{fontSize:10,color:diff>=0?"#059669":"#DC2626"}}>{diff>=0?"+":""}{diff}pts</span>}<span style={{fontSize:12,fontWeight:700,color:percCo(g2)}}>{g2}%</span></div></div>})}</div>}
          {recA.length>0&&<div style={{...cd(),padding:14,marginBottom:12}}><h3 style={{fontFamily:FD,fontSize:13,margin:"0 0 6px"}}>Actividades recomendadas</h3>{recA.map((a,i)=>{const d=DOM.find(x=>x.k===a.dom);const Ic=d?.I;return <div key={i} onClick={()=>setMdl({t:"actD",a})} style={{display:"flex",alignItems:"center",gap:8,padding:"5px 0",borderBottom:"1px solid "+BL,cursor:"pointer"}}>{Ic&&<div style={{width:24,height:24,borderRadius:6,background:d.c+"15",display:"flex",alignItems:"center",justifyContent:"center"}}><Ic size={12} color={d.c}/></div>}<div style={{flex:1}}><div style={{fontSize:12,fontWeight:500}}>{a.t}</div><div style={{fontSize:10,color:TM}}>{a.obj} · {a.dur}</div></div></div>})}</div>}
          {sMil.length>0&&<div style={{...cd(),padding:14,marginBottom:12}}><h3 style={{fontFamily:FD,fontSize:13,margin:"0 0 6px"}}>{"Hitos alcanzados"}</h3>{sMil.map(m=>{const d=DOM.find(x=>x.k===m.dom);return <div key={m.id} style={{display:"flex",gap:6,padding:"5px 0",borderBottom:"1px solid "+BL}}><Award size={14} color={d?.c}/><div><div style={{fontSize:12,fontWeight:500}}>{m.text}</div><div style={{fontSize:10,color:TL}}>{fmt(m.dt)} · {d?.l}</div></div></div>})}</div>}
          {sObs.length>0&&<div style={{...cd(),padding:14}}><h3 style={{fontFamily:FD,fontSize:13,margin:"0 0 6px"}}>{"Últimas observaciones"}</h3>{sObs.slice(-4).reverse().map(o=>{const d=DOM.find(x=>x.k===o.dom);return <div key={o.id} style={{padding:6,borderLeft:"3px solid "+(o.tp==="positive"?"#10B981":o.tp==="concern"?"#F59E0B":BD),background:BG,borderRadius:8,marginBottom:4}}><div style={{display:"flex",justifyContent:"space-between"}}><span style={{fontSize:10,color:TL}}>{fmt(o.dt)}</span>{d&&<Pill t={d.l} co={d.c} bg={d.c+"15"}/>}</div><div style={{fontSize:12,marginTop:2}}>{o.text}</div></div>})}</div>}
        </div>}
        {/* TAB: Domains with per-domain percentiles */}
        {stTab==="domains"&&ev&&<div>
          {DOM.map(d=>{const val=ev.sc[d.k]||0;const Ic=d.I;const dp=getDomPctl(d.k,s.dob);const sev=val<dp.P10?"danger":val<dp.P25?"warning":val>dp.P90?"excellent":"normal";const prev=prevEv?prevEv.sc[d.k]||0:null;const diff=prev!==null?val-prev:null;return <div key={d.k} style={{...cd(),padding:14,marginBottom:8,borderLeft:"3px solid "+sevCo(sev)}}>
            <div style={{display:"flex",justifyContent:"space-between",alignItems:"center",marginBottom:6}}>
              <span style={{display:"flex",alignItems:"center",gap:5,fontSize:14,fontWeight:700}}><div style={{width:28,height:28,borderRadius:8,background:d.c+"15",display:"flex",alignItems:"center",justifyContent:"center"}}><Ic size={14} color={d.c}/></div>{d.l}</span>
              <div style={{display:"flex",alignItems:"center",gap:6}}>{diff!==null&&<span style={{fontSize:11,color:diff>0?"#059669":"#DC2626",fontWeight:600}}>{diff>0?"+":""}{diff}{diff>0?<ArrowUp size={10}/>:<ArrowDown size={10}/>}</span>}<span style={{fontWeight:800,color:sevCo(sev),fontSize:16,fontFamily:FD}}>{val}%</span></div>
            </div>
            <div style={{fontSize:11,color:TM,marginBottom:6}}>Subáreas: {d.sub.join(" · ")}</div>
            <div style={{display:"flex",gap:4,fontSize:10,color:TM,flexWrap:"wrap",marginBottom:6}}>
              <span style={{padding:"2px 6px",borderRadius:4,background:val<dp.P10?"#FEE2E2":"#fff",border:"1px solid "+BD}}>P10: {dp.P10}</span>
              <span style={{padding:"2px 6px",borderRadius:4,background:val<dp.P25?"#FEF3C7":"#fff",border:"1px solid "+BD}}>P25: {dp.P25}</span>
              <span style={{padding:"2px 6px",borderRadius:4,background:"#fff",border:"1px solid "+BD,fontWeight:700}}>P50: {dp.P50}</span>
              <span style={{padding:"2px 6px",borderRadius:4,background:val>dp.P90?"#D1FAE5":"#fff",border:"1px solid "+BD}}>P90: {dp.P90}</span>
              <span style={{marginLeft:"auto",fontWeight:700,color:sevCo(sev),fontSize:11}}>{sev==="danger"?"< P10":sev==="warning"?"P10-P25":sev==="excellent"?"> P90":"P25-P90"}</span>
            </div>
            <div style={{height:6,borderRadius:99,background:BL,position:"relative"}}><div style={{height:"100%",borderRadius:99,background:d.c,width:val+"%"}}/><div style={{position:"absolute",top:-2,left:dp.P50+"%",width:2,height:10,background:TM,borderRadius:1}}/><div style={{position:"absolute",top:-1,left:dp.P10+"%",width:1,height:8,background:"#FCA5A5",borderRadius:1}}/><div style={{position:"absolute",top:-1,left:dp.P25+"%",width:1,height:8,background:"#FCD34D",borderRadius:1}}/></div>
          </div>})}
          {/* Age-specific percentile reference table */}
          <div style={{...cd(),padding:14}}><h3 style={{fontFamily:FD,fontSize:13,margin:"0 0 8px"}}>Tabla de percentiles — Edad: {getAgeY(s.dob)} años</h3>
            <div style={{overflowX:"auto"}}><table style={{width:"100%",borderCollapse:"collapse",fontSize:11}}>
              <thead><tr style={{background:BL}}><td style={{padding:6,fontWeight:600}}>Dominio</td><td style={{padding:6,textAlign:"center"}}>P10</td><td style={{padding:6,textAlign:"center"}}>P25</td><td style={{padding:6,textAlign:"center",fontWeight:700,color:P}}>P50</td><td style={{padding:6,textAlign:"center"}}>P90</td><td style={{padding:6,textAlign:"center",fontWeight:700}}>Score</td><td style={{padding:6,textAlign:"center"}}>Estado</td></tr></thead>
              <tbody>{DOM.map(d=>{const val=ev.sc[d.k]||0;const dp=getDomPctl(d.k,s.dob);const sev=val<dp.P10?"danger":val<dp.P25?"warning":val>dp.P90?"excellent":"normal";return <tr key={d.k} style={{borderBottom:"1px solid "+BL}}><td style={{padding:6,display:"flex",alignItems:"center",gap:4}}><d.I size={11} color={d.c}/>{d.l}</td><td style={{padding:6,textAlign:"center",color:val<dp.P10?"#DC2626":TM}}>{dp.P10}</td><td style={{padding:6,textAlign:"center",color:val<dp.P25?"#D97706":TM}}>{dp.P25}</td><td style={{padding:6,textAlign:"center",fontWeight:600}}>{dp.P50}</td><td style={{padding:6,textAlign:"center",color:val>dp.P90?"#059669":TM}}>{dp.P90}</td><td style={{padding:6,textAlign:"center",fontWeight:700,color:sevCo(sev)}}>{val}</td><td style={{padding:6,textAlign:"center"}}><Pill t={sev==="danger"?"Urgente":sev==="warning"?"Atención":sev==="excellent"?"Excelente":"Normal"} co={sevCo(sev)} bg={sevBg(sev)}/></td></tr>})}</tbody>
            </table></div>
          </div>
        </div>}
        {!ev&&stTab==="domains"&&<div style={{...cd(),padding:30,textAlign:"center"}}><p style={{color:TL}}>Evalúa al alumno para ver los dominios detallados</p></div>}
        {/* TAB: Timeline */}
        {stTab==="timeline"&&<div>
          {(()=>{const events=[];sEvs.forEach(e=>events.push({dt:e.dt,type:"eval",data:e}));sObs.forEach(o=>events.push({dt:o.dt,type:"obs",data:o}));sMil.forEach(m=>events.push({dt:m.dt,type:"mile",data:m}));sAA.forEach(a=>events.push({dt:a.dt,type:"act",data:a}));sInc.forEach(i=>events.push({dt:i.dt,type:"inc",data:i}));sPn.forEach(n=>events.push({dt:n.dt,type:"note",data:n}));sAls.forEach(a=>events.push({dt:td(),type:"alert",data:a}));events.sort((a,b)=>b.dt.localeCompare(a.dt));
          return events.length===0?<div style={{...cd(),padding:30,textAlign:"center"}}><p style={{color:TL}}>Sin eventos</p></div>:events.map((ev2,i)=>{
            const ico=ev2.type==="eval"?{I:Clipboard,c:P,l:"Evaluación"}:ev2.type==="obs"?{I:Edit3,c:ev2.data.tp==="positive"?"#10B981":ev2.data.tp==="concern"?"#D97706":TM,l:"Observación"}:ev2.type==="mile"?{I:Award,c:"#8B5CF6",l:"Hito"}:ev2.type==="act"?{I:Play,c:"#0EA5E9",l:"Actividad"}:ev2.type==="inc"?{I:AlertTriangle,c:"#EA580C",l:"Incidencia"}:ev2.type==="note"?{I:Send,c:"#3B82F6",l:"Nota familia"}:{I:Bell,c:"#DC2626",l:"Alerta"};
            const Ic=ico.I;
            return <div key={i} style={{display:"flex",gap:10,marginBottom:2}}>
              <div style={{display:"flex",flexDirection:"column",alignItems:"center"}}><div style={{width:28,height:28,borderRadius:99,background:ico.c+"15",display:"flex",alignItems:"center",justifyContent:"center",flexShrink:0}}><Ic size={13} color={ico.c}/></div>{i<events.length-1&&<div style={{width:2,flex:1,background:BD,minHeight:16}}/>}</div>
              <div style={{flex:1,paddingBottom:8}}><div style={{display:"flex",justifyContent:"space-between",alignItems:"center"}}><span style={{fontSize:11,fontWeight:600,color:ico.c}}>{ico.l}</span><span style={{fontSize:10,color:TL}}>{fmt(ev2.dt)}</span></div>
                {ev2.type==="eval"&&<div style={{fontSize:11,color:TM}}>Score global: <span style={{fontWeight:700,color:percCo(gs(ev2.data.sc))}}>{gs(ev2.data.sc)}%</span></div>}
                {ev2.type==="obs"&&<div style={{fontSize:11,color:TM}}>{ev2.data.text}</div>}
                {ev2.type==="mile"&&<div style={{fontSize:11,color:TM}}>{ev2.data.text}</div>}
                {ev2.type==="act"&&<div style={{fontSize:11,color:TM}}>{ev2.data.act} {ev2.data.done?<Pill t="Completada" co="#059669" bg="#ECFDF5"/>:<Pill t="Pendiente" co="#D97706" bg="#FFFBEB"/>}</div>}
                {ev2.type==="inc"&&<div style={{fontSize:11,color:TM}}>{ev2.data.text}</div>}
                {ev2.type==="note"&&<div style={{fontSize:11,color:TM}}>{ev2.data.text}</div>}
                {ev2.type==="alert"&&<div style={{fontSize:11,color:TM}}>{ev2.data.msg}</div>}
              </div>
            </div>;
          });})()}
        </div>}
        {/* TAB: Activities */}
        {stTab==="activities"&&<div>
          <div style={{display:"flex",justifyContent:"space-between",marginBottom:8}}><h3 style={{fontFamily:FD,fontSize:14,margin:0}}>Actividades asignadas ({sAA.length})</h3><button onClick={()=>setMdl({t:"assign",s})} style={bp(true)}>+ Asignar</button></div>
          {sAA.length===0?<div style={{...cd(),padding:20,textAlign:"center"}}><p style={{color:TL,fontSize:12}}>Sin actividades asignadas</p></div>:sAA.sort((a,b)=>b.dt.localeCompare(a.dt)).map(a=>{const d=DOM.find(x=>x.k===a.dom);return <div key={a.id} style={{...cd(),padding:10,marginBottom:4,borderLeft:"3px solid "+(a.done?"#10B981":"#D97706")}}>
            <div style={{display:"flex",justifyContent:"space-between",alignItems:"center"}}><div><div style={{fontSize:12,fontWeight:600}}>{a.act}</div><div style={{fontSize:10,color:TM}}>{d?.l} · {fmt(a.dt)}</div></div><div style={{display:"flex",gap:3}}>{!a.done&&<button onClick={()=>up({actAssign:(D.actAssign||[]).map(x=>x.id===a.id?{...x,done:true}:x)})} style={bp(true)}>{"Completar"}</button>}<Pill t={a.done?"Hecho":"Pendiente"} co={a.done?"#059669":"#D97706"} bg={a.done?"#ECFDF5":"#FFFBEB"}/></div></div>
            {a.notes&&<div style={{fontSize:11,color:TM,marginTop:3}}>{a.notes}</div>}
          </div>})}
        </div>}
        {/* TAB: PMI */}
        {stTab==="pmi"&&<div>
          <div style={{display:"flex",justifyContent:"space-between",marginBottom:8}}><h3 style={{fontFamily:FD,fontSize:14,margin:0}}>Planes de Mejora Individual</h3><button onClick={()=>setMdl({t:"pmi",s})} style={bp(true)}>+ Nuevo PMI</button></div>
          {sPmi.length===0?<div style={{...cd(),padding:20,textAlign:"center"}}><p style={{color:TL,fontSize:12}}>Sin planes de mejora</p></div>:sPmi.map(pm=>{const d=DOM.find(x=>x.k===pm.dom);return <div key={pm.id} style={{...cd(),padding:12,marginBottom:6}}>
            <div style={{display:"flex",justifyContent:"space-between",marginBottom:4}}><span style={{fontSize:13,fontWeight:600,color:d?.c}}>{d?.l}</span><div style={{display:"flex",gap:4}}><Pill t={pm.status==="active"?"Activo":"Cerrado"} co={pm.status==="active"?P:TM} bg={pm.status==="active"?PL:BL}/>{pm.status==="active"&&<button onClick={()=>up({pmi:(D.pmi||[]).map(x=>x.id===pm.id?{...x,pr:Math.min(100,x.pr+10)}:x)})} style={bo(false,true)}>+10%</button>}</div></div>
            <div style={{height:6,borderRadius:99,background:BL,marginBottom:6}}><div style={{height:"100%",borderRadius:99,background:d?.c||P,width:pm.pr+"%"}}/></div>
            <div style={{fontSize:11,color:TM,marginBottom:2}}>Progreso: {pm.pr}%</div>
            {(pm.obj||[]).map((o,i)=><div key={i} style={{fontSize:12,color:TX,padding:"2px 0"}}>· {o}</div>)}
            <div style={{fontSize:10,color:TL,marginTop:4}}>Inicio: {fmt(pm.dt)}{pm.rev?" · Revisión: "+fmt(pm.rev):""}</div>
          </div>})}
        </div>}
        {/* TAB: Incidents */}
        {stTab==="incidents"&&<div>
          <div style={{display:"flex",justifyContent:"space-between",marginBottom:8}}><h3 style={{fontFamily:FD,fontSize:14,margin:0}}>Incidencias ({sInc.length})</h3><button onClick={()=>setMdl({t:"inc",s})} style={bp(true)}>+ Registrar</button></div>
          {sInc.length===0?<div style={{...cd(),padding:20,textAlign:"center"}}><p style={{color:TL,fontSize:12}}>Sin incidencias registradas</p></div>:sInc.sort((a,b)=>b.dt.localeCompare(a.dt)).map(inc=>{const co=inc.sev==="high"?"#DC2626":inc.sev==="medium"?"#EA580C":"#D97706";return <div key={inc.id} style={{...cd(),padding:10,marginBottom:4,borderLeft:"3px solid "+co}}>
            <div style={{display:"flex",justifyContent:"space-between"}}><div style={{display:"flex",gap:4}}><Pill t={inc.type} co={co} bg={co+"12"}/><Pill t={inc.sev==="high"?"Alta":inc.sev==="medium"?"Media":"Baja"} co={co} bg={co+"12"}/></div><span style={{fontSize:10,color:TL}}>{fmt(inc.dt)}</span></div>
            <div style={{fontSize:12,color:TX,marginTop:4}}>{inc.text}</div>
          </div>})}
          {sPn.length>0&&<div style={{marginTop:12}}><h3 style={{fontFamily:FD,fontSize:13,margin:"0 0 6px"}}>Notas a familia ({sPn.length})</h3>{sPn.map(n=>{const co=n.type==="derivacion"?"#DC2626":n.type==="seguimiento"?"#D97706":n.type==="reunion"?"#3B82F6":n.type==="felicitacion"?"#059669":P;return <div key={n.id} style={{padding:6,background:co+"08",borderLeft:"3px solid "+co,borderRadius:8,marginBottom:4}}><div style={{display:"flex",justifyContent:"space-between"}}><Pill t={n.type||"info"} co={co} bg={co+"15"}/><span style={{fontSize:10,color:TL}}>{fmt(n.dt)}</span></div><div style={{fontSize:12,marginTop:2}}>{n.text}</div></div>})}</div>}
        </div>}
      </div>;
    }
    /* STUDENT LIST */
    if(v==="sts"){return <div><div style={{display:"flex",justifyContent:"space-between",marginBottom:12}}><h2 style={{fontFamily:FD,fontSize:20,margin:0}}>{"Alumnos"}</h2><button onClick={()=>setMdl("addSt")} style={bp(true)}>{"+ "+"Alumnos"}</button></div>{sts.length===0?<div style={{...cd(),padding:30,textAlign:"center"}}><p style={{color:TL}}>Sin alumnos</p></div>:<div style={{display:"grid",gridTemplateColumns:"repeat(auto-fill,minmax(220px,1fr))",gap:8}}>{sts.map(s=>{const ev=le(s.id);const pctl=getPctl(s.dob);const sAls2=(D.als||[]).filter(a=>a.sid===s.id&&a.st==="active");return <div key={s.id} onClick={()=>setSel(s.id)} style={{...cd(),padding:12,cursor:"pointer",borderLeft:sAls2.length?"3px solid "+(sAls2.some(a=>a.sev==="high")?"#EF4444":"#F59E0B"):"3px solid transparent"}}><div style={{display:"flex",alignItems:"center",gap:8,marginBottom:6}}><Av nm={s.nm} sz={30}/><div><div style={{fontSize:12,fontWeight:600}}>{s.nm}</div><div style={{fontSize:10,color:TL}}>{ageFn(s.dob)}{sAls2.length?" · "+sAls2.length+" "+"alerta"+(sAls2.length>1?"s":""):""}</div></div></div>{ev?<Bars sc={ev.sc} sm={true}/>:<div style={{fontSize:11,color:TL,textAlign:"center"}}>Sin evaluar</div>}</div>})}</div>}</div>;}
    /* EVAL PICKER */
    if(v==="eval"){const [evalMode,setEvalMode]=[stTab==="guided"?"guided":"standard",v2=>setStTab(v2==="guided"?"guided":"overview")];return <div><h2 style={{fontFamily:FD,fontSize:20,margin:"0 0 4px"}}>Evaluar alumno</h2><p style={{fontSize:12,color:TM,margin:"0 0 10px"}}>Elige el modo de evaluación y selecciona un alumno.</p>
      <div style={{display:"flex",gap:4,marginBottom:12}}><button onClick={()=>setStTab("overview")} style={bo(evalMode==="standard")}><Clipboard size={13} style={{verticalAlign:"middle",marginRight:3}}/>Evaluación completa</button><button onClick={()=>setStTab("guided")} style={bo(evalMode==="guided")}><Play size={13} style={{verticalAlign:"middle",marginRight:3}}/>Evaluación guiada</button></div>
      {evalMode==="standard"&&<div style={{padding:10,background:PL,borderRadius:8,marginBottom:10}}><div style={{fontSize:12,fontWeight:600,color:PD}}>Evaluación completa NEXUS™</div><div style={{fontSize:11,color:TM}}>30 ítems en 6 dominios. Evaluación global del alumno (10 min).</div></div>}
      {evalMode==="guided"&&<div style={{padding:10,background:"#FFF7ED",borderRadius:8,marginBottom:10}}><div style={{fontSize:12,fontWeight:600,color:"#D97706"}}>Evaluación guiada por actividades</div><div style={{fontSize:11,color:TM}}>Realiza una actividad con el alumno y evalúa lo observado. El sistema calcula el resultado por dominio automáticamente.</div></div>}
      {sts.length===0?<p style={{color:TL}}>Añade alumnos primero</p>:<div style={{display:"grid",gridTemplateColumns:"repeat(auto-fill,minmax(180px,1fr))",gap:8}}>{sts.map(s=>{const ev=le(s.id);return <button key={s.id} onClick={()=>setMdl(evalMode==="guided"?{t:"guided",s}:{t:"eval",s})} style={{...cd(),padding:12,cursor:"pointer",textAlign:"left"}}><div style={{display:"flex",alignItems:"center",gap:7}}><Av nm={s.nm} sz={28}/><div><span style={{fontSize:12,fontWeight:600,display:"block"}}>{s.nm}</span><span style={{fontSize:10,color:TL}}>{ev?"Última evaluación"+": "+fmt(ev.dt):"Sin evaluar"}</span></div></div></button>})}</div>}</div>;}
    /* ANALYSIS */
    if(v==="analysis"){
      const evSts=sts.filter(s=>le(s.id));
      const pctl=getPctl("2021-06-01");
      return <div>
        <h2 style={{fontFamily:FD,fontSize:20,margin:"0 0 4px"}}>Análisis del aula</h2>
        <p style={{fontSize:12,color:TM,margin:"0 0 12px"}}>Distribución por percentiles, análisis por dominio y screening · {evSts.length} evaluados</p>
        <div style={{display:"flex",gap:4,marginBottom:12,flexWrap:"wrap"}}>{[{k:"pctl",l:"Percentiles"},{k:"domain",l:"Por dominio"},{k:"screening",l:"Screening"},{k:"daily",l:"Análisis diario"}].map(t=><button key={t.k} onClick={()=>setAnalTab(t.k)} style={bo(analTab===t.k,true)}>{t.l}</button>)}</div>
        {analTab==="pctl"&&<div>
          <div style={{...cd(),padding:14,marginBottom:12}}><h3 style={{fontFamily:FD,fontSize:14,margin:"0 0 10px"}}>Distribución por percentiles</h3>
            <div style={{display:"grid",gridTemplateColumns:"1fr 1fr 1fr",gap:6,marginBottom:10}}>{[{l:"Sobre P90",c:"#059669",bg:"#ECFDF5",cnt:evSts.filter(s=>gs(le(s.id).sc)>pctl.P90).length},{l:"P25-P90",c:P,bg:PL,cnt:evSts.filter(s=>{const g2=gs(le(s.id).sc);return g2>=pctl.P25&&g2<=pctl.P90;}).length},{l:"Bajo P25",c:"#DC2626",bg:"#FEF2F2",cnt:evSts.filter(s=>gs(le(s.id).sc)<pctl.P25).length}].map((x,i)=><div key={i} style={{...cd(),padding:10,background:x.bg,textAlign:"center"}}><div style={{fontSize:20,fontWeight:800,color:x.c,fontFamily:FD}}>{x.cnt}</div><div style={{fontSize:10,color:x.c}}>{x.l}</div></div>)}</div>
            <div style={{fontSize:12,fontWeight:600,color:TX,marginBottom:6}}>Tabla de percentiles de referencia</div>
            <div style={{display:"grid",gridTemplateColumns:"repeat(7,1fr)",gap:2}}>{["P5","P10","P25","P50","P75","P90","P95"].map(p=><div key={p} style={{padding:6,background:p==="P50"?P:BL,borderRadius:6,textAlign:"center"}}><div style={{fontSize:9,color:p==="P50"?"#fff":TM,fontWeight:600}}>{p}</div><div style={{fontSize:12,fontWeight:700,color:p==="P50"?"#fff":TX}}>{pctl[p]}</div></div>)}</div>
          </div>
          {evSts.map(s=>{const ev=le(s.id);const g2=gs(ev.sc);const sev=sevLb(g2,pctl);return <div key={s.id} style={{...cd(),padding:10,marginBottom:6,cursor:"pointer",borderLeft:"3px solid "+sevCo(sev)}} onClick={()=>{setSel(s.id);setV("sts");}}><div style={{display:"flex",alignItems:"center",gap:8}}><Av nm={s.nm} sz={28}/><div style={{flex:1}}><div style={{fontSize:12,fontWeight:600}}>{s.nm}</div><div style={{fontSize:10,color:TL}}>{ageFn(s.dob)} · {pctlLabel(g2,pctl)}</div></div><div style={{textAlign:"right"}}><div style={{fontSize:14,fontWeight:700,color:sevCo(sev)}}>{g2}%</div><div style={{fontSize:9,color:TM}}>{percLb(g2)}</div></div></div></div>})}
        </div>}
        {analTab==="domain"&&<div>
          {DOM.map(d=>{const vals=evSts.map(s=>(le(s.id).sc[d.k]||0));const avg=vals.length?Math.round(vals.reduce((a,b)=>a+b,0)/vals.length):0;const minV=vals.length?Math.min(...vals):0;const maxV=vals.length?Math.max(...vals):0;const low=vals.filter(v2=>v2<pctl.P25).length;const Ic=d.I;return <div key={d.k} style={{...cd(),padding:14,marginBottom:8}}>
            <div style={{display:"flex",alignItems:"center",gap:6,marginBottom:8}}><div style={{width:32,height:32,borderRadius:8,background:d.c+"15",display:"flex",alignItems:"center",justifyContent:"center"}}><Ic size={16} color={d.c}/></div><div style={{flex:1}}><div style={{fontSize:14,fontWeight:600}}>{d.l}</div><div style={{fontSize:10,color:TM}}>Subáreas: {d.sub.join(", ")}</div></div><div style={{textAlign:"right"}}><div style={{fontSize:16,fontWeight:800,color:percCo(avg),fontFamily:FD}}>{avg}%</div><div style={{fontSize:9,color:TM}}>Media</div></div></div>
            <div style={{display:"flex",gap:8,marginBottom:6}}><div style={{flex:1,padding:6,background:BL,borderRadius:6,textAlign:"center"}}><div style={{fontSize:11,fontWeight:700,color:TX}}>{minV}%</div><div style={{fontSize:9,color:TM}}>Mínimo</div></div><div style={{flex:1,padding:6,background:BL,borderRadius:6,textAlign:"center"}}><div style={{fontSize:11,fontWeight:700,color:TX}}>{maxV}%</div><div style={{fontSize:9,color:TM}}>Máximo</div></div><div style={{flex:1,padding:6,background:low>0?"#FEF2F2":BL,borderRadius:6,textAlign:"center"}}><div style={{fontSize:11,fontWeight:700,color:low>0?"#DC2626":TX}}>{low}</div><div style={{fontSize:9,color:TM}}>Bajo P25</div></div></div>
            <div style={{height:5,borderRadius:99,background:BL}}><div style={{height:"100%",borderRadius:99,background:d.c,width:avg+"%"}}/></div>
          </div>})}
        </div>}
        {/* SCREENING - Risk Matrix */}
        {analTab==="screening"&&<div>
          <div style={{...cd(),padding:14,marginBottom:12}}><h3 style={{fontFamily:FD,fontSize:14,margin:"0 0 4px"}}>Matriz de Screening</h3><p style={{fontSize:11,color:TM,margin:"0 0 10px"}}>Detección temprana por dominio · Percentiles P10 y P25 como umbrales</p>
            <div style={{overflowX:"auto"}}><table style={{width:"100%",borderCollapse:"collapse",fontSize:11}}>
              <thead><tr style={{background:BL}}><td style={{padding:6,fontWeight:600}}>Alumno</td>{DOM.map(d=><td key={d.k} style={{padding:6,textAlign:"center"}}><d.I size={11} color={d.c} style={{verticalAlign:"middle"}}/> {d.l.slice(0,4)}</td>)}<td style={{padding:6,textAlign:"center",fontWeight:600}}>Global</td></tr></thead>
              <tbody>{evSts.map(s=>{const ev=le(s.id);const g2=gs(ev.sc);return <tr key={s.id} style={{borderBottom:"1px solid "+BL,cursor:"pointer"}} onClick={()=>{setSel(s.id);setV("sts");}}>
                <td style={{padding:6,fontWeight:500}}>{s.nm.split(" ")[0]}</td>
                {DOM.map(d=>{const val=ev.sc[d.k]||0;const dp=getDomPctl(d.k,s.dob);const bg=val<dp.P10?"#FEE2E2":val<dp.P25?"#FEF3C7":val>dp.P90?"#D1FAE5":"#fff";const co=val<dp.P10?"#DC2626":val<dp.P25?"#D97706":val>dp.P90?"#059669":TX;return <td key={d.k} style={{padding:6,textAlign:"center",background:bg,color:co,fontWeight:val<dp.P25?700:400}}>{val}</td>})}
                <td style={{padding:6,textAlign:"center",fontWeight:700,color:percCo(g2)}}>{g2}</td>
              </tr>})}</tbody>
            </table></div>
          </div>
          {/* Risk summary */}
          <div style={{display:"grid",gridTemplateColumns:"1fr 1fr",gap:8}}>
            <div style={{...cd(),padding:12,borderLeft:"4px solid #DC2626"}}><h4 style={{fontSize:12,fontWeight:700,color:"#DC2626",margin:"0 0 6px"}}>Derivación urgente (P10)</h4>
              {evSts.filter(s=>{const ev=le(s.id);return DOM.some(d=>{const dp=getDomPctl(d.k,s.dob);return (ev.sc[d.k]||0)<dp.P10;});}).map(s=>{const ev=le(s.id);const weakDoms=DOM.filter(d=>(ev.sc[d.k]||0)<getDomPctl(d.k,s.dob).P10);return <div key={s.id} style={{fontSize:11,padding:"3px 0"}}><span style={{fontWeight:600}}>{s.nm}</span>: {weakDoms.map(d=>d.l).join(", ")}</div>})}
              {evSts.filter(s=>{const ev=le(s.id);return DOM.some(d=>(ev.sc[d.k]||0)<getDomPctl(d.k,s.dob).P10);}).length===0&&<div style={{fontSize:11,color:TL}}>Ninguno</div>}
            </div>
            <div style={{...cd(),padding:12,borderLeft:"4px solid #D97706"}}><h4 style={{fontSize:12,fontWeight:700,color:"#D97706",margin:"0 0 6px"}}>Seguimiento (P25)</h4>
              {evSts.filter(s=>{const ev=le(s.id);return DOM.some(d=>{const dp=getDomPctl(d.k,s.dob);const val=ev.sc[d.k]||0;return val>=dp.P10&&val<dp.P25;});}).map(s=>{const ev=le(s.id);const weakDoms=DOM.filter(d=>{const val=ev.sc[d.k]||0;const dp=getDomPctl(d.k,s.dob);return val>=dp.P10&&val<dp.P25;});return <div key={s.id} style={{fontSize:11,padding:"3px 0"}}><span style={{fontWeight:600}}>{s.nm}</span>: {weakDoms.map(d=>d.l).join(", ")}</div>})}
            </div>
          </div>
        </div>}
        {analTab==="daily"&&<div>
          <div style={{display:"flex",justifyContent:"flex-end",marginBottom:8}}><button onClick={()=>setMdl("dailyAn")} style={bp(true)}>+ Nuevo análisis</button></div>
          {(D.dailyAn||[]).length===0?<div style={{...cd(),padding:30,textAlign:"center"}}><p style={{color:TL}}>Sin análisis diarios</p></div>:(D.dailyAn||[]).map((da,i)=><div key={i} style={{...cd(),padding:14,marginBottom:8}}>
            <div style={{display:"flex",justifyContent:"space-between",alignItems:"center",marginBottom:6}}><div><div style={{fontSize:13,fontWeight:600}}>{fmt(da.dt)}</div><div style={{fontSize:11,color:TM}}>Foco: {da.focus}</div></div>{da.domFocus&&<div style={{display:"flex",gap:4}}>{Object.entries(da.domFocus).map(([k,v2])=>{const d=DOM.find(x=>x.k===k);return d?<Pill key={k} t={d.l+": "+v2+"%"} co={d.c} bg={d.c+"15"}/>:null;})}</div>}</div>
            <p style={{fontSize:12,color:TX,margin:"0 0 6px"}}>{da.summary}</p>
            {da.highlights&&<div style={{marginBottom:4}}>{da.highlights.map((h,j)=><div key={j} style={{fontSize:11,color:"#059669",padding:"1px 0"}}><CheckCircle size={9} style={{verticalAlign:"middle",marginRight:3}}/>{h}</div>)}</div>}
            {da.concerns&&da.concerns.length>0&&<div>{da.concerns.map((c,j)=><div key={j} style={{fontSize:11,color:"#D97706",padding:"1px 0"}}><AlertTriangle size={9} style={{verticalAlign:"middle",marginRight:3}}/>{c}</div>)}</div>}
          </div>)}
        </div>}
      </div>;
    }
    /* ACTIVITIES */
    if(v==="acts"){const all=allActs(D);const [actSearch,setActSearch]=[stTab.startsWith("s:")?stTab.slice(2):"",v2=>setStTab("s:"+v2)];const [actLvl,setActLvl2]=[actF.startsWith("L")?parseInt(actF[1]):0,v2=>setActF(v2?"L"+v2:"all")];const [actSub,setActSub2]=[actF.startsWith("S:")?actF.slice(2):"",v2=>setActF(v2?"S:"+v2:"all")];const domF=(!actF.startsWith("L")&&!actF.startsWith("S:")&&actF!=="all"&&actF!=="fav"&&actF!=="custom")?actF:"";const isFav=actF==="fav";const isCustom=actF==="custom";const favs=D.favActs||[];const toggleFav=(t)=>{const nf=favs.includes(t)?favs.filter(x=>x!==t):[...favs,t];up({favActs:nf});};
      let fActs=all;
      if(domF)fActs=fActs.filter(a=>a.dom===domF);
      if(actLvl)fActs=fActs.filter(a=>(a.lvl||1)===actLvl);
      if(actSub)fActs=fActs.filter(a=>a.sub===actSub);
      if(isFav)fActs=fActs.filter(a=>favs.includes(a.t));
      if(isCustom)fActs=(D.customActs||[]);
      if(actSearch)fActs=fActs.filter(a=>(a.t+a.desc+a.obj+a.sub+(a.tags||[]).join(" ")).toLowerCase().includes(actSearch.toLowerCase()));
      const curDom=domF?DOM.find(d=>d.k===domF):null;const subs=curDom?ACT_SUBS[domF]||[]:[];
      const usedCount=(D.actAssign||[]).length;const completedCount=(D.actAssign||[]).filter(a=>a.done).length;
      return <div><div style={{display:"flex",justifyContent:"space-between",alignItems:"flex-start",marginBottom:4}}><div><h2 style={{fontFamily:FD,fontSize:20,margin:"0 0 4px"}}>Catálogo de actividades</h2><p style={{fontSize:12,color:TM,margin:"0 0 4px"}}>{all.length} actividades · {usedCount} asignadas · {completedCount} completadas</p></div><button onClick={()=>setMdl("createAct")} style={bp()}>+ Crear actividad</button></div>
      <input value={actSearch} onChange={e=>setActSearch(e.target.value)} placeholder="Buscar actividad, dominio, objetivo, tag..." style={{...inp,marginBottom:8,background:"#fff"}}/>
      <div style={{display:"flex",gap:3,marginBottom:6,flexWrap:"wrap"}}><button onClick={()=>{setActF("all");setActSearch("");}} style={bo(actF==="all",true)}>Todas ({all.length})</button>{DOM.map(d=>{const cnt=all.filter(a=>a.dom===d.k).length;return <button key={d.k} onClick={()=>setActF(d.k)} style={{...bo(domF===d.k,true),color:domF===d.k?"#fff":d.c,background:domF===d.k?d.c:d.c+"12",borderColor:d.c}}>{d.l} ({cnt})</button>})}<button onClick={()=>setActF("fav")} style={{...bo(isFav,true),color:isFav?"#fff":"#F59E0B",background:isFav?"#F59E0B":"#FFFBEB",borderColor:"#F59E0B"}}>★ Favoritas ({favs.length})</button>{(D.customActs||[]).length>0&&<button onClick={()=>setActF("custom")} style={{...bo(isCustom,true),color:isCustom?"#fff":"#8B5CF6",background:isCustom?"#8B5CF6":"#F5F3FF",borderColor:"#8B5CF6"}}>Propias ({(D.customActs||[]).length})</button>}</div>
      {domF&&<div style={{display:"flex",gap:3,marginBottom:6,flexWrap:"wrap"}}><button onClick={()=>setActLvl2(0)} style={bo(!actLvl,true)}>Todos los niveles</button>{[1,2,3].map(l=><button key={l} onClick={()=>setActLvl2(l)} style={{...bo(actLvl===l,true),background:actLvl===l?[P,"#F59E0B","#DC2626"][l-1]:"#fff",color:actLvl===l?"#fff":[P,"#D97706","#DC2626"][l-1],borderColor:[P,"#F59E0B","#DC2626"][l-1]}}>Nivel {l} {l===1?"★":l===2?"★★":"★★★"}</button>)}{subs.length>0&&<span style={{fontSize:11,color:TM,padding:"5px 0"}}>|</span>}{subs.map(s=><button key={s} onClick={()=>actSub===s?setActSub2(""):setActSub2(s)} style={bo(actSub===s,true)}>{s}</button>)}</div>}
      {domF&&<div style={{...cd(),padding:12,marginBottom:10,background:curDom.c+"08",borderLeft:"4px solid "+curDom.c}}><div style={{display:"flex",justifyContent:"space-between",alignItems:"center"}}><div><span style={{fontSize:13,fontWeight:700,color:curDom.c}}>{curDom.l}</span><span style={{fontSize:11,color:TM,marginLeft:6}}>Progresión por sub-objetivo</span></div></div>
      <div style={{display:"grid",gridTemplateColumns:"repeat(auto-fill,minmax(200px,1fr))",gap:6,marginTop:8}}>{(ACT_SUBS[domF]||[]).map(sub=>{const chain=all.filter(a=>a.dom===domF&&a.sub===sub).sort((a,b)=>(a.lvl||1)-(b.lvl||1));return <div key={sub} style={{padding:8,background:"#fff",borderRadius:8,border:"1px solid "+BD}}><div style={{fontSize:11,fontWeight:600,color:TX,marginBottom:4}}>{sub}</div><div style={{display:"flex",gap:2}}>{chain.map((a,i)=><div key={i} style={{flex:1,textAlign:"center"}}><div style={{width:20,height:20,borderRadius:99,background:a.lvl===1?P+"25":a.lvl===2?"#F59E0B25":"#DC262625",display:"flex",alignItems:"center",justifyContent:"center",fontSize:8,fontWeight:700,color:a.lvl===1?P:a.lvl===2?"#D97706":"#DC2626",margin:"0 auto 2px"}}>{a.lvl||1}</div><div style={{fontSize:8,color:TM,lineHeight:1.2}}>{a.t.length>18?a.t.slice(0,16)+"…":a.t}</div></div>)}</div></div>})}</div></div>}
      <div style={{fontSize:11,color:TM,marginBottom:6}}>{fActs.length} actividad{fActs.length!==1?"es":""} encontrada{fActs.length!==1?"s":""}</div>
      <div style={{display:"grid",gridTemplateColumns:"repeat(auto-fill,minmax(260px,1fr))",gap:8}}>{fActs.map((a,i)=>{const d=DOM.find(x=>x.k===a.dom);const Ic=d?.I;const isFaved=favs.includes(a.t);const nxt=nextLvlAct(a,D);const assigned=(D.actAssign||[]).filter(aa=>aa.act===a.t).length;return <div key={i} style={{...cd(),padding:12,position:"relative"}}><div style={{position:"absolute",top:6,right:6,display:"flex",gap:3}}><button onClick={e=>{e.stopPropagation();toggleFav(a.t);}} style={{background:"none",border:"none",cursor:"pointer",fontSize:14,padding:0}}>{isFaved?"★":"☆"}</button></div><div onClick={()=>setMdl({t:"actD",a})} style={{cursor:"pointer"}}><div style={{display:"flex",alignItems:"center",gap:5,marginBottom:4}}>{Ic&&<Ic size={14} color={d.c}/>}<span style={{fontSize:13,fontWeight:600}}>{a.t}</span></div><div style={{fontSize:11,color:TM,marginBottom:6,lineHeight:1.4}}>{a.desc.slice(0,90)}{a.desc.length>90?"…":""}</div><div style={{display:"flex",gap:3,flexWrap:"wrap",marginBottom:4}}><Pill t={"Nv."+((a.lvl||1))} co={(a.lvl||1)===1?P:(a.lvl||1)===2?"#D97706":"#DC2626"} bg={(a.lvl||1)===1?PL:(a.lvl||1)===2?"#FFFBEB":"#FEF2F2"}/><Pill t={a.dur} co={TM} bg={BL}/><Pill t={a.age} co={TM} bg={BL}/>{a.sub&&<Pill t={a.sub} co={d?.c} bg={d?.c+"15"}/>}</div>{a.tags&&a.tags.length>0&&<div style={{display:"flex",gap:2,flexWrap:"wrap",marginBottom:4}}>{a.tags.map((tg,j)=><span key={j} style={{fontSize:8,padding:"1px 5px",borderRadius:99,background:BL,color:TM}}>#{tg}</span>)}</div>}{assigned>0&&<div style={{fontSize:10,color:P}}>Asignada {assigned}x</div>}{nxt&&<div style={{fontSize:9,color:TM,marginTop:2}}>Siguiente nivel → {nxt.t}</div>}</div></div>})}</div>
    </div>;}
    /* JOURNAL */
    if(v==="journal"){return <div><div style={{display:"flex",justifyContent:"space-between",marginBottom:12}}><div><h2 style={{fontFamily:FD,fontSize:20,margin:0}}>Diario de aula</h2><p style={{fontSize:12,color:TM,margin:"2px 0 0"}}>Registro diario del clima y actividad del aula</p></div><button onClick={()=>setMdl("journal")} style={bp()}>+ Nuevo registro</button></div>
      {(D.journal||[]).length===0?<div style={{...cd(),padding:30,textAlign:"center"}}><BookOpen size={30} color={BD}/><p style={{color:TL}}>Sin registros aún</p></div>:(D.journal||[]).map(j=><div key={j.id} style={{...cd(),padding:12,marginBottom:6}}>
        <div style={{display:"flex",justifyContent:"space-between",marginBottom:4}}><span style={{fontSize:12,fontWeight:600}}>{fmt(j.dt)}</span><div style={{display:"flex",gap:4}}>{j.attendance!=null&&<Pill t={j.attendance+" "+"asist."} co={TM} bg={BL}/>}<Pill t={j.climate||"Normal"} co={j.climate==="Excelente"||j.climate==="Tranquilo"?"#059669":j.climate==="Agitado"||j.climate==="Difícil"?"#DC2626":TM} bg={j.climate==="Excelente"||j.climate==="Tranquilo"?"#ECFDF5":j.climate==="Agitado"||j.climate==="Difícil"?"#FEF2F2":BL}/><Pill t={j.mood==="positive"?"Positivo":j.mood==="concern"?"Preocupante":"Neutral"} co={j.mood==="positive"?"#059669":j.mood==="concern"?"#DC2626":TM} bg={j.mood==="positive"?"#ECFDF5":j.mood==="concern"?"#FEF2F2":BL}/></div></div>
        <div style={{fontSize:12,color:TX,lineHeight:1.5,marginBottom:4}}>{j.text}</div>
        {j.acts&&j.acts.length>0&&<div style={{display:"flex",gap:3,flexWrap:"wrap"}}>{j.acts.map((a,i)=><Pill key={i} t={a} co={P} bg={PL}/>)}</div>}
      </div>)}</div>;}
    /* ALERTS */
    if(v==="alerts"){return <div><h2 style={{fontFamily:FD,fontSize:20,margin:"0 0 12px"}}>Alertas ({aAlerts.length})</h2>{aAlerts.length===0?<div style={{...cd(),padding:30,textAlign:"center"}}><Bell size={30} color={BD}/><p style={{color:TL}}>Sin alertas activas</p></div>:aAlerts.map(a=>{const s2=(D.sts||[]).find(x=>x.id===a.sid);const d=DOM.find(x=>x.k===a.dom);return <div key={a.id} style={{...cd(),padding:12,marginBottom:6,borderLeft:"4px solid "+(a.sev==="high"?"#EF4444":"#F59E0B")}}><div style={{display:"flex",justifyContent:"space-between",alignItems:"center"}}><div><div style={{fontSize:13,fontWeight:600}}>{s2?.nm}</div><div style={{fontSize:11,color:d?.c}}>{d?.l} · {a.sev==="high"?"Urgente — Derivación":"Atención — Seguimiento"}</div></div><div style={{display:"flex",gap:4}}><button onClick={()=>{setSel(s2?.id);setV("sts");}} style={bo(false,true)}>Ver perfil</button><button onClick={()=>up({als:(D.als||[]).map(x=>x.id===a.id?{...x,st:"resolved"}:x)})} style={bp(true)}>Resolver</button></div></div><div style={{fontSize:12,color:TM,marginTop:3}}>{a.msg}</div></div>})}</div>;}
    /* TRACKING */
    if(v==="track"){const tracked=sts.filter(s=>(D.als||[]).some(a=>a.sid===s.id&&a.st==="active")||(D.pmi||[]).some(p=>p.sid===s.id&&p.status==="active"));return <div><h2 style={{fontFamily:FD,fontSize:20,margin:"0 0 4px"}}>Seguimiento</h2><p style={{fontSize:12,color:TM,margin:"0 0 12px"}}>Alumnos con alertas activas o planes de mejora</p>
      {tracked.length===0?<div style={{...cd(),padding:30,textAlign:"center"}}><Target size={30} color={BD}/><p style={{color:TL}}>Sin alumnos en seguimiento</p></div>:tracked.map(s=>{const ev=le(s.id);const pctl2=getPctl(s.dob);const sAls=(D.als||[]).filter(a=>a.sid===s.id&&a.st==="active");const sPmi2=(D.pmi||[]).filter(p=>p.sid===s.id&&p.status==="active");const sAA2=(D.actAssign||[]).filter(a=>a.sid===s.id);const aaDone=sAA2.filter(a=>a.done).length;const sObs=(D.obs||[]).filter(o=>o.sid===s.id).slice(-2);return <div key={s.id} style={{...cd(),padding:14,marginBottom:8}}>
        <div style={{display:"flex",alignItems:"center",gap:8,marginBottom:8,cursor:"pointer"}} onClick={()=>{setSel(s.id);setV("sts");}}><Av nm={s.nm} sz={36}/><div style={{flex:1}}><div style={{fontSize:14,fontWeight:600}}>{s.nm}</div><div style={{fontSize:11,color:TL}}>{ageFn(s.dob)}</div></div>{ev&&<div style={{textAlign:"right"}}><div style={{fontSize:18,fontWeight:800,color:percCo(gs(ev.sc)),fontFamily:FD}}>{gs(ev.sc)}%</div><div style={{fontSize:9,color:TM}}>{percLb(gs(ev.sc))}</div></div>}</div>
        {sAls.length>0&&<div style={{marginBottom:6}}>{sAls.map(a=>{const d=DOM.find(x=>x.k===a.dom);return <div key={a.id} style={{display:"flex",alignItems:"center",gap:4,padding:"3px 0"}}><div style={{width:6,height:6,borderRadius:99,background:a.sev==="high"?"#EF4444":"#F59E0B"}}/><span style={{fontSize:11,color:TM}}><span style={{fontWeight:600,color:d?.c}}>{d?.l}</span> {a.msg}</span></div>})}</div>}
        {sPmi2.length>0&&<div style={{marginBottom:4}}>{sPmi2.map(pm=>{const d=DOM.find(x=>x.k===pm.dom);return <div key={pm.id} style={{padding:6,background:BL,borderRadius:6,marginBottom:3}}><div style={{display:"flex",justifyContent:"space-between"}}><span style={{fontSize:11,fontWeight:600,color:d?.c}}>{d?.l} — PMI</span><Pill t={pm.pr+"%"} co={percCo(pm.pr)} bg={percBg(pm.pr)}/></div>{(pm.obj||[]).map((o,i)=><div key={i} style={{fontSize:10,color:TM}}>· {o}</div>)}</div>})}</div>}
        {sAA2.length>0&&<div style={{fontSize:10,color:TM,marginBottom:4}}>Actividades: {aaDone}/{sAA2.length} completadas</div>}
        {sObs.length>0&&<div style={{paddingTop:4,borderTop:"1px solid "+BL}}><div style={{fontSize:10,fontWeight:600,color:TM,marginBottom:2}}>{"Últimas observaciones"}</div>{sObs.map(o=><div key={o.id} style={{fontSize:10,color:TM,padding:"1px 0"}}>{fmt(o.dt)}: {o.text.slice(0,60)}...</div>)}</div>}
      </div>})}</div>;}
    /* REPORTS */
    /* REPORT PDF */
    if(pdfSid){
      const pdfS=sts.find(x=>x.id===pdfSid);
      const pdfEv=pdfS?le(pdfS.id):null;
      if(pdfS&&pdfEv){
        const g2r=gs(pdfEv.sc);const ageR2=getAgeRange(pdfS.dob);
        const rAls=(D.als||[]).filter(a=>a.sid===pdfS.id&&a.st==="active");
        const rPmi=(D.pmi||[]).filter(p=>p.sid===pdfS.id&&p.status==="active");
        const rMil=(D.milestones||[]).filter(m=>m.sid===pdfS.id);
        const rObs=(D.obs||[]).filter(o=>o.sid===pdfS.id).slice(-5);
        const rEvs=allEvs(pdfS.id);const rPrev=rEvs.length>1?rEvs[rEvs.length-2]:null;
        const dC={cog:"#0D9488",emo:"#EC4899",soc:"#3B82F6",mot:"#F59E0B",sen:"#8B5CF6",mtv:"#059669"};
        const dN={cog:"Cognitivo",emo:"Emocional",soc:"Social",mot:"Motor",sen:"Sensorial",mtv:"Motivacional"};
        const dE={cog:"🧠",emo:"💗",soc:"👥",mot:"⚡",sen:"👁",mtv:"💡"};
        const pRx=PMI_RECS||{};const now=new Date();
        const per=(now.getDate()<=15?"1-15 ":"16-"+new Date(now.getFullYear(),now.getMonth()+1,0).getDate()+" ")+now.toLocaleDateString("es-ES",{month:"long"});
        const gC=g2r>=75?"#059669":g2r>=50?"#0D9488":g2r>=25?"#D97706":"#DC2626";
        const pLx=(v,dk)=>{const dp=getDomPctl(dk,pdfS.dob);if(v<dp.P10)return["Bajo P10","#DC2626","#FEF2F2"];if(v<dp.P25)return["P10-P25","#D97706","#FFFBEB"];if(v>dp.P90)return["Sobre P90","#059669","#ECFDF5"];return["Adecuado","#0D9488","#F0FDFA"];};
        const hSt={fontFamily:FD,fontSize:15,color:P,margin:"16px 0 8px",paddingBottom:4,borderBottom:"2px solid #E2E8F0"};
        return <div style={{position:"fixed",inset:0,zIndex:9999,background:"#fff",display:"flex",flexDirection:"column"}}>
          <div style={{display:"flex",alignItems:"center",justifyContent:"space-between",padding:"8px 16px",background:DARK,color:"#fff",flexShrink:0}}>
            <div style={{display:"flex",alignItems:"center",gap:8}}><div style={{width:28,height:28,borderRadius:99,background:P,display:"flex",alignItems:"center",justifyContent:"center",fontFamily:FD,fontWeight:700,fontSize:14}}>N</div><span style={{fontFamily:FD,fontSize:14}}>{"Informe Quincenal de Desarrollo"} — {pdfS.nm}</span></div>
            <div style={{display:"flex",gap:6}}>
              <button onClick={()=>window.print()} style={{...bp(),padding:"6px 14px",fontSize:11,display:"flex",alignItems:"center",gap:4}}><Printer size={13}/>{"Imprimir / Guardar PDF"}</button>
              <button onClick={()=>setPdfSid(null)} style={{background:"rgba(255,255,255,.15)",color:"#fff",border:"none",borderRadius:8,padding:"6px 12px",cursor:"pointer",fontSize:11,display:"flex",alignItems:"center",gap:4}}><X size={13}/>{"Cerrar"}</button>
            </div>
          </div>
          <div style={{flex:1,overflow:"auto",background:"#fff",padding:"0 16px 40px",maxWidth:850,margin:"0 auto",width:"100%",fontSize:11,lineHeight:1.6}}>
            <div style={{background:"linear-gradient(135deg,#0C3C36,#134E4A)",color:"#fff",padding:"20px 24px",borderRadius:"0 0 12px 12px",marginBottom:16}}>
              <div style={{float:"right",width:42,height:42,borderRadius:"50%",background:P,display:"flex",alignItems:"center",justifyContent:"center",fontFamily:FD,fontWeight:700,fontSize:22,color:"#fff"}}>N</div>
              <h1 style={{fontFamily:FD,fontSize:21,color:"#fff",margin:"0 0 2px"}}>{"Informe Quincenal de Desarrollo"}</h1>
              <div style={{color:"#A7F3D0",fontSize:12}}>NEXUS KIDS</div>
              <div style={{display:"flex",gap:14,marginTop:8,fontSize:10,color:"#D1FAE5",flexWrap:"wrap"}}>
                <span>{"Alumno"+": "}<b>{pdfS.nm}</b></span>
                <span>{"Edad"+": "}<b>{ageFn(pdfS.dob)}</b>{" ("+ageR2+" años)"}</span>
                <span>{"Periodo"+": "}<b>{per+" "+now.getFullYear()}</b></span>
                <span>{"Fecha: "}<b>{now.toLocaleDateString("es-ES")}</b></span>
              </div>
            </div>
            <h2 style={hSt}>{"Perfil Global"}</h2>
            <div style={{textAlign:"center",padding:8}}>
              <span style={{fontFamily:FD,fontSize:40,fontWeight:"bold",color:gC}}>{g2r}%</span>
              <div style={{fontSize:12,color:"#64748B"}}>{g2r>=75?"Excelente":g2r>=50?"Adecuado":g2r>=25?"Requiere atención":"Intervención necesaria"}</div>
            </div>
            <div style={{display:"grid",gridTemplateColumns:"1fr 1fr 1fr",gap:8,margin:"12px 0"}}>
              {DOM.map(d=>{const val=pdfEv.sc[d.k]||0;const pl=pLx(val,d.k);const prev=rPrev?(rPrev.sc[d.k]||0):null;const diff=prev!==null?val-prev:null;
                return <div key={d.k} style={{borderRadius:8,padding:10,textAlign:"center",border:"1px solid #E2E8F0",background:"#fff",borderTop:"3px solid "+dC[d.k]}}>
                  <div style={{fontSize:14}}>{dE[d.k]}</div>
                  <div style={{fontSize:12,fontWeight:600,color:dC[d.k]}}>{dN[d.k]}</div>
                  <div style={{fontFamily:FD,fontSize:26,fontWeight:"bold",color:pl[1]}}>{val}%</div>
                  <div style={{height:5,background:"#E2E8F0",borderRadius:99,margin:"5px 0"}}><div style={{height:"100%",borderRadius:99,width:val+"%",background:dC[d.k]}}/></div>
                  <span style={{display:"inline-block",padding:"2px 7px",borderRadius:99,fontSize:9,fontWeight:600,background:pl[2],color:pl[1]}}>{pl[0]}</span>
                  {diff!==null&&<div style={{fontSize:10,marginTop:3,color:diff>=0?"#059669":"#DC2626"}}>{(diff>=0?"+":"")+diff+" "+"vs anterior"}</div>}
                </div>;})}
            </div>
            <h2 style={hSt}>{"Análisis por Dominio"}</h2>
            {DOM.map(d=>{const val=pdfEv.sc[d.k]||0;const dp=getDomPctl(d.k,pdfS.dob);const pl=pLx(val,d.k);
              const bdr=val<dp.P10?"#DC2626":val<dp.P25?"#D97706":val>dp.P90?"#059669":"#0D9488";
              const bgc=val<dp.P10?"#FEF2F2":val<dp.P25?"#FFFBEB":val>dp.P90?"#ECFDF5":"#F0FDFA";
              const dAl=rAls.filter(a=>a.dom===d.k);
              const dPm=rPmi.filter(p=>p.dom===d.k);
              const dMl=rMil.filter(m=>m.dom===d.k);
              const recs=((pRx[d.k]||{})[ageR2])||[];
              let actR=[];try{actR=(allActs(D)||[]).filter(a=>a.dom===d.k).slice(0,2);}catch(e){}
              return <div key={d.k} style={{background:bgc,border:"1px solid #E2E8F0",borderLeft:"4px solid "+bdr,borderRadius:8,padding:10,margin:"6px 0"}}>
                <div style={{display:"flex",justifyContent:"space-between",alignItems:"center"}}>
                  <div>{dE[d.k]+" "}<b style={{color:dC[d.k],fontSize:13}}>{dN[d.k]}</b></div>
                  <div><span style={{fontFamily:FD,fontSize:18,fontWeight:"bold",color:pl[1]}}>{val}%</span><span style={{display:"inline-block",padding:"2px 7px",borderRadius:99,fontSize:9,fontWeight:600,background:pl[2],color:pl[1],marginLeft:4}}>{pl[0]}</span></div>
                </div>
                <div style={{fontSize:10,color:"#64748B",margin:"5px 0"}}>{"P10="+dp.P10+" P25="+dp.P25+" P50="+dp.P50+" P90="+dp.P90}</div>
                {dAl.length>0&&<div style={{margin:"5px 0",padding:"5px 7px",background:"#FEF2F2",borderRadius:4,fontSize:10,color:"#DC2626"}}>{"⚠ Alertas: "+dAl.length}</div>}
                {dPm.map((p,pi)=><div key={pi} style={{margin:"4px 0",fontSize:10}}><b>{"PMI activo: "}</b>{(p.obj||[]).map((o,oi)=><div key={oi} style={{marginLeft:8}}>{"· "+o}</div>)}</div>)}
                {dMl.length>0&&<div style={{margin:"4px 0",fontSize:10}}>{"🌟 Hitos: "+dMl.map(m=>m.text).join(", ")}</div>}
                {val<dp.P50&&recs.length>0&&<div style={{marginTop:7}}>
                  <b style={{fontSize:10,color:"#0D9488"}}>{"Recomendaciones ("+ageR2+" años):"}</b>
                  {recs.slice(0,3).map((r,ri)=><div key={ri} style={{padding:"6px 9px",background:val<dp.P10?"#FEF2F2":val<dp.P25?"#FFFBEB":"#F0FDFA",borderLeft:"3px solid "+(val<dp.P10?"#DC2626":val<dp.P25?"#F59E0B":"#0D9488"),margin:"3px 0",borderRadius:"0 6px 6px 0",fontSize:10}}>{"→ "+r}</div>)}
                  {actR.length>0&&<div style={{marginTop:4,fontSize:10,color:"#0D9488"}}><b>{"Actividades: "}</b>{actR.map(a=>(a.t||"")+" (Nv."+(a.lvl||1)+")").join(" / ")}</div>}
                </div>}
                {val>=dp.P50&&<div style={{padding:"6px 9px",background:"#ECFDF5",borderLeft:"3px solid #059669",margin:"3px 0",borderRadius:"0 6px 6px 0",fontSize:10}}>{"✓ "+"Desarrollo adecuado o superior."}</div>}
              </div>;})}
            {rEvs.length>1&&<div>
              <h2 style={hSt}>{"Evolución"}</h2>
              <table style={{width:"100%",borderCollapse:"collapse",fontSize:10}}>
                <thead><tr><th style={{background:"#F1F5F9",padding:5,textAlign:"left",borderBottom:"2px solid #E2E8F0"}}>Fecha</th>
                  {DOM.map(d=><th key={d.k} style={{background:"#F1F5F9",padding:5,color:dC[d.k],borderBottom:"2px solid #E2E8F0"}}>{dN[d.k]}</th>)}<th style={{background:"#F1F5F9",padding:5,borderBottom:"2px solid #E2E8F0"}}>Global</th></tr></thead>
                <tbody>{rEvs.slice(-6).map((e,ei)=><tr key={ei}><td style={{padding:4,borderBottom:"1px solid #F1F5F9"}}>{(e.dt||"").slice(0,10)}</td>
                  {DOM.map(d=>{const v2=e.sc[d.k]||0;const dp2=getDomPctl(d.k,pdfS.dob);return <td key={d.k} style={{padding:4,borderBottom:"1px solid #F1F5F9",color:v2<dp2.P10?"#DC2626":v2<dp2.P25?"#D97706":"#1E293B"}}>{v2}%</td>;})}<td style={{padding:4,borderBottom:"1px solid #F1F5F9",fontWeight:"bold"}}>{gs(e.sc)}%</td></tr>)}</tbody>
              </table>
            </div>}
            {rAls.length>0&&<div><h2 style={hSt}>{"Alertas Activas"}</h2>
              {rAls.map((a,ai)=>{const dk=a.dom||"cog";return <div key={ai} style={{background:a.sev==="high"?"#FEF2F2":"#FFFBEB",borderLeft:"4px solid "+(a.sev==="high"?"#DC2626":"#F59E0B"),borderRadius:8,padding:10,margin:"6px 0"}}><b>{dN[dk]||dk}</b>{" — "}{a.sev==="high"?"Derivación":"Seguimiento"}<div style={{fontSize:10,color:"#64748B"}}>{a.msg||""}</div></div>;})}</div>}
            {rPmi.length>0&&<div><h2 style={hSt}>{"Planes de Mejora"}</h2>
              {rPmi.map((p,pi)=>{const dk=p.dom||"cog";return <div key={pi} style={{background:"#F8FFFE",border:"1px solid #E2E8F0",borderRadius:8,padding:10,margin:"6px 0"}}>
                <div style={{display:"flex",justifyContent:"space-between"}}><b style={{color:dC[dk]||P}}>{(dN[dk]||"")+" — PMI"}</b><span style={{display:"inline-block",padding:"2px 7px",borderRadius:99,fontSize:9,fontWeight:600,background:"#FFFBEB",color:"#D97706"}}>{"Progreso: "+(p.pr||0)+"%"}</span></div>
                {(p.obj||[]).map((o,oi)=><div key={oi} style={{fontSize:10,padding:"2px 0"}}>{"· "+o}</div>)}</div>;})}</div>}
            <h2 style={hSt}>{"Plan de Acción Quincenal"}</h2>
            <div style={{background:"#F0FDFA",border:"1px solid #E2E8F0",borderLeft:"4px solid #0D9488",borderRadius:8,padding:10,margin:"6px 0"}}>
              {(()=>{const weak=DOM.filter(d=>(pdfEv.sc[d.k]||0)<(getDomPctl(d.k,pdfS.dob).P50));
                if(!weak.length)return <div style={{padding:10,textAlign:"center",color:"#059669"}}>{"✓ "+"Todos los dominios dentro de lo esperado."}</div>;
                return weak.map(d=>{const val=pdfEv.sc[d.k]||0;const recs=((pRx[d.k]||{})[ageR2])||[];let aR=[];try{aR=(allActs(D)||[]).filter(a=>a.dom===d.k).slice(0,2);}catch(e){}
                  return <div key={d.k} style={{margin:"8px 0",padding:10,background:"#fff",borderRadius:6,border:"1px solid #E2E8F0"}}>
                    <b style={{color:dC[d.k]}}>{dE[d.k]+" "+dN[d.k]+" — "+val+"%"}</b>
                    {recs.length>0&&<div style={{marginTop:5}}><b style={{fontSize:10}}>{"Objetivos ("+ageR2+" años):"}</b>{recs.slice(0,3).map((r,ri)=><div key={ri} style={{fontSize:10,margin:"2px 0 2px 8px"}}>{"→ "+r}</div>)}</div>}
                    {aR.length>0&&<div style={{marginTop:4,fontSize:10}}><b>{"Actividades: "}</b>{aR.map((a,ai)=><div key={ai} style={{margin:"2px 0 2px 8px"}}>{(a.t||"")+" (Nv."+(a.lvl||1)+")"}</div>)}</div>}
                  </div>;});})()}
            </div>
            {rObs.length>0&&<div><h2 style={hSt}>{"Observaciones recientes"}</h2>
              {rObs.map((o,oi)=>{const dk=o.dom||"cog";return <div key={oi} style={{fontSize:10,color:"#64748B",padding:"4px 0",borderBottom:"1px solid #F1F5F9"}}><span style={{color:dC[dk]||"#94A3B8",fontWeight:600}}>{(dN[dk]||"")+":"}</span>{" "+(o.text||"")}</div>;})}</div>}
            {rMil.length>0&&<div><h2 style={hSt}>{"Hitos Alcanzados"}</h2>
              <div style={{display:"grid",gridTemplateColumns:"1fr 1fr",gap:6}}>{rMil.map((m,mi)=>{const dk=m.dom||"cog";return <div key={mi} style={{background:"#F8FFFE",border:"1px solid #E2E8F0",borderRadius:8,padding:8}}>{(dE[dk]||"")+" "}<b>{m.text||""}</b><div style={{fontSize:9,color:"#94A3B8"}}>{(dN[dk]||"")+" — "+(m.dt||"").slice(0,10)}</div></div>;})}</div></div>}
            <div style={{textAlign:"center",fontSize:9,color:"#94A3B8",marginTop:20,paddingTop:10,borderTop:"1px solid #E2E8F0"}}>
              <div style={{marginBottom:3,fontSize:11,fontWeight:600,color:P}}>NEXUS KIDS</div>
              {"Informe generado el"+" "+now.toLocaleDateString("",{day:"numeric",month:"long",year:"numeric"})+" — "+"Documento confidencial."}<br/>
              {"Los percentiles se basan en el Framework NEXUS y no sustituyen una evaluación clínica profesional."}
            </div>
          </div>
        </div>;
      }
    }

    if(v==="reports"){
      const evSts=sts.filter(s=>le(s.id));
      return <div><h2 style={{fontFamily:FD,fontSize:20,margin:"0 0 4px"}}>Informes</h2><p style={{fontSize:12,color:TM,margin:"0 0 12px"}}>Generación de informes individuales y de aula</p>
        <div style={{display:"flex",gap:4,marginBottom:12}}>{[{k:"individual",l:"Individual"},{k:"aula",l:"De aula"},{k:"screening",l:"Screening"}].map(t=><button key={t.k} onClick={()=>setRepTab(t.k)} style={bo(repTab===t.k,true)}>{t.l}</button>)}</div>
        {repTab==="individual"&&<div>
          <p style={{fontSize:12,color:TM,margin:"0 0 10px"}}>{"Selecciona un alumno para generar su informe quincenal"}</p>
          {evSts.map(s=>{const ev=le(s.id);const g2=gs(ev.sc);const pctl2=getPctl(s.dob);const sAls=(D.als||[]).filter(a=>a.sid===s.id&&a.st==="active");const sPn=(D.pNotes||[]).filter(n=>n.sid===s.id);const sEvs2=allEvs(s.id);const sMil=(D.milestones||[]).filter(m=>m.sid===s.id);
          return <div key={s.id} style={{...cd(),padding:14,marginBottom:8}}>
            <div style={{display:"flex",alignItems:"center",gap:8,marginBottom:10}}><Av nm={s.nm} sz={32}/><div style={{flex:1}}><div style={{fontSize:13,fontWeight:600}}>{s.nm}</div><div style={{fontSize:11,color:TL}}>{ageFn(s.dob)} · {sEvs2.length} evaluaciones</div></div><div style={{textAlign:"right"}}><div style={{fontSize:18,fontWeight:800,color:percCo(g2),fontFamily:FD}}>{g2}%</div><div style={{fontSize:9,color:TM}}>{percLb(g2)}</div></div></div>
            <div style={{background:BL,borderRadius:10,padding:12,marginBottom:8}}>
              <div style={{fontSize:11,fontWeight:600,marginBottom:6}}>{"Resumen del informe quincenal"}</div>
              {DOM.map(d=>{const val=ev.sc[d.k]||0;const dp=getDomPctl(d.k,s.dob);const sev=val<dp.P10?"danger":val<dp.P25?"warning":val>dp.P90?"excellent":"normal";return <div key={d.k} style={{display:"flex",justifyContent:"space-between",padding:"3px 0",fontSize:11}}><span style={{display:"flex",alignItems:"center",gap:3}}><d.I size={10} color={d.c}/>{d.l}</span><span style={{fontWeight:600,color:sevCo(sev)}}>{val}% ({sev==="danger"?"<P10":sev==="warning"?"<P25":sev==="excellent"?">P90":"OK"})</span></div>})}
            </div>
            {sAls.length>0&&<div style={{fontSize:11,color:"#DC2626",marginBottom:4}}>{"Alertas"}: {sAls.length} {"activas"} — {sAls.filter(a=>a.sev==="high").length} {"urgentes"}</div>}
            {sMil.length>0&&<div style={{fontSize:11,color:"#8B5CF6",marginBottom:4}}>{"Hitos"}: {sMil.length} {"alcanzados"}</div>}
            <div style={{fontSize:11,color:TM,marginBottom:8}}>{"Notas a familia"}: {sPn.length} · {"Observaciones"}: {(D.obs||[]).filter(o=>o.sid===s.id).length}</div>
            <button onClick={()=>setPdfSid(s.id)} style={{...bp(),width:"100%",display:"flex",alignItems:"center",justifyContent:"center",gap:6,padding:"10px 16px",fontSize:12,fontWeight:600}}><FileText size={14}/>{"Generar Informe Quincenal PDF"}</button>
          </div>})}
        </div>}
        {repTab==="aula"&&<div>
          <div style={{...cd(),padding:14,marginBottom:12}}>
            <h3 style={{fontFamily:FD,fontSize:14,margin:"0 0 10px"}}>{"Informe de aula"} — {myCls[0]?.name||""}</h3>
            <div style={{display:"grid",gridTemplateColumns:"repeat(3,1fr)",gap:8,marginBottom:12}}>{[{l:"Alumnos evaluados",v:evSts.length+"/"+sts.length},{l:"Alertas activas",v:aAlerts.length},{l:"PMIs activos",v:(D.pmi||[]).filter(p=>p.status==="active"&&sts.some(s=>s.id===p.sid)).length}].map((k,i)=><div key={i} style={{padding:8,background:BL,borderRadius:8,textAlign:"center"}}><div style={{fontSize:16,fontWeight:700,fontFamily:FD}}>{k.v}</div><div style={{fontSize:10,color:TM}}>{k.l}</div></div>)}</div>
            <h4 style={{fontSize:12,fontWeight:600,margin:"0 0 6px"}}>{"Promedios por dominio"}</h4>
            {DOM.map(d=>{const vals=evSts.map(s=>(le(s.id).sc[d.k]||0));const avg=vals.length?Math.round(vals.reduce((a,b)=>a+b,0)/vals.length):0;return <div key={d.k} style={{display:"flex",alignItems:"center",gap:6,padding:"4px 0"}}><d.I size={12} color={d.c}/><span style={{fontSize:11,flex:1}}>{d.l}</span><div style={{width:100,height:5,borderRadius:99,background:BL}}><div style={{height:"100%",borderRadius:99,background:d.c,width:avg+"%"}}/></div><span style={{fontSize:11,fontWeight:700,color:percCo(avg),width:35,textAlign:"right"}}>{avg}%</span></div>})}
          </div>
          <div style={{...cd(),padding:14}}>
            <h4 style={{fontSize:12,fontWeight:600,margin:"0 0 6px"}}>{"Alumnos en seguimiento prioritario"}</h4>
            {evSts.filter(s=>gs(le(s.id).sc)<getPctl(s.dob).P25).map(s=>{const ev=le(s.id);const g2=gs(ev.sc);return <div key={s.id} style={{display:"flex",alignItems:"center",gap:6,padding:"4px 0",borderBottom:"1px solid "+BL}}><Av nm={s.nm} sz={22}/><span style={{fontSize:11,flex:1}}>{s.nm}</span><span style={{fontSize:11,fontWeight:700,color:percCo(g2)}}>{g2}%</span></div>})}
            {evSts.filter(s=>gs(le(s.id).sc)<getPctl(s.dob).P25).length===0&&<div style={{fontSize:11,color:TL}}>Ningún alumno por debajo de P25</div>}
          </div>
        </div>}
        {repTab==="screening"&&<div>
          <div style={{...cd(),padding:14}}><h3 style={{fontFamily:FD,fontSize:14,margin:"0 0 4px"}}>{"Informe de screening semestral"}</h3><p style={{fontSize:11,color:TM,margin:"0 0 10px"}}>{"Visión global para dirección"+" · "+new Date().toLocaleDateString("",{month:"long",year:"numeric"})}</p>
            <div style={{display:"grid",gridTemplateColumns:"repeat(4,1fr)",gap:6,marginBottom:12}}>{[{l:"Evaluados",v:evSts.length,c:P},{l:"Sin alertas",v:evSts.filter(s=>!(D.als||[]).some(a=>a.sid===s.id&&a.st==="active")).length,c:"#059669"},{l:"En seguimiento",v:evSts.filter(s=>(D.als||[]).some(a=>a.sid===s.id&&a.st==="active"&&a.sev==="medium")).length,c:"#D97706"},{l:"Derivados",v:evSts.filter(s=>(D.als||[]).some(a=>a.sid===s.id&&a.st==="active"&&a.sev==="high")).length,c:"#DC2626"}].map((k,i)=><div key={i} style={{padding:8,background:k.c+"08",borderRadius:8,textAlign:"center"}}><div style={{fontSize:18,fontWeight:800,color:k.c,fontFamily:FD}}>{k.v}</div><div style={{fontSize:9,color:k.c}}>{k.l}</div></div>)}</div>
            <div style={{fontSize:11,color:TM}}>{"Tasa de detección"}: {evSts.length?Math.round((D.als||[]).filter(a=>a.st==="active"&&a.sev==="high"&&sts.some(s=>s.id===a.sid)).length/evSts.length*100):0}% {"Derivados"} · {evSts.length?Math.round((D.als||[]).filter(a=>a.st==="active"&&sts.some(s=>s.id===a.sid)).length/evSts.length*100):0}% {"con alguna alerta"}</div>
          </div>
        </div>}
      </div>;
    }
    /* MSGS / FAMILIES */
    if(v==="msgs"){const myMsgs=(D.msgs||[]).filter(m=>m.from===auth.uid||m.to===auth.uid);const parentNotes=(D.pNotes||[]).filter(n=>n.from===auth.uid);const myInvs=(D.invitations||[]).filter(i=>i.tid===auth.uid);const pendInvs=myInvs.filter(i=>i.status==="pending");const accInvs=myInvs.filter(i=>i.status==="accepted");const stsNoInv=sts.filter(s=>!myInvs.find(i=>i.sid===s.id));return <div><div style={{display:"flex",justifyContent:"space-between",marginBottom:12}}><div><h2 style={{fontFamily:FD,fontSize:20,margin:0}}>{"Comunicación con familias"}</h2><p style={{fontSize:12,color:TM,margin:"2px 0 0"}}>{accInvs.length} {"vinculados"} · {pendInvs.length} {"pendientes"} · {stsNoInv.length} {"sin invitar"}</p></div></div>
      <div style={{display:"grid",gridTemplateColumns:"repeat(3,1fr)",gap:8,marginBottom:12}}><div style={{...cd(),padding:10,textAlign:"center",borderTop:"3px solid #059669"}}><div style={{fontFamily:FD,fontSize:20,color:"#059669"}}>{accInvs.length}</div><div style={{fontSize:10,color:TM}}>{"Familias vinculadas"}</div></div><div style={{...cd(),padding:10,textAlign:"center",borderTop:"3px solid #F59E0B"}}><div style={{fontFamily:FD,fontSize:20,color:"#D97706"}}>{pendInvs.length}</div><div style={{fontSize:10,color:TM}}>{"pendientes"}</div></div><div style={{...cd(),padding:10,textAlign:"center",borderTop:"3px solid #DC2626"}}><div style={{fontFamily:FD,fontSize:20,color:"#DC2626"}}>{stsNoInv.length}</div><div style={{fontSize:10,color:TM}}>{"sin invitar"}</div></div></div>
      <div style={{...cd(),padding:12,marginBottom:12}}><div style={{display:"flex",justifyContent:"space-between",alignItems:"center",marginBottom:8}}><h3 style={{fontFamily:FD,fontSize:13,margin:0}}>{"Estado de vinculación"}</h3></div>
      <div style={{display:"grid",gridTemplateColumns:"repeat(auto-fill,minmax(200px,1fr))",gap:6}}>{sts.map(s=>{const inv=myInvs.find(i=>i.sid===s.id);const par=inv?(D.parents||[]).find(p=>p.id===inv.parId):null;const linked=inv?.status==="accepted"&&par;const pending=inv?.status==="pending";return <div key={s.id} style={{padding:8,borderRadius:10,background:linked?PL:pending?"#FFFBEB":BG,border:"1.5px solid "+(linked?P:pending?"#F59E0B":BD)}}><div style={{display:"flex",alignItems:"center",gap:6,marginBottom:4}}><Av nm={s.nm} sz={22}/><div><div style={{fontSize:11,fontWeight:600}}>{s.nm}</div>{linked&&<div style={{fontSize:10,color:"#059669"}}>✓ {par.nm}</div>}{pending&&<div style={{fontSize:10,color:"#D97706"}}>{"⏳ Código: "}<b style={{fontFamily:"monospace",letterSpacing:1}}>{inv.code}</b></div>}{!inv&&<div style={{fontSize:10,color:"#DC2626"}}>{"Sin invitación"}</div>}</div></div>{!inv&&<button onClick={()=>setMdl({t:"invite",s})} style={{...bo(false,true),width:"100%",fontSize:10,padding:4,background:"#EFF6FF",borderColor:"#3B82F6",color:"#2563EB"}}><UserPlus size={9}/>{" Invitar"}</button>}</div>})}</div></div>
      <div style={{...cd(),padding:12,marginBottom:12}}><h3 style={{fontFamily:FD,fontSize:13,margin:"0 0 8px"}}>{"Enviar nota rápida"}</h3><div style={{display:"grid",gridTemplateColumns:"repeat(auto-fill,minmax(160px,1fr))",gap:6}}>{sts.map(s=><button key={s.id} onClick={()=>setMdl({t:"pnote",s})} style={{...cd(),padding:8,cursor:"pointer",textAlign:"left"}}><div style={{display:"flex",alignItems:"center",gap:5}}><Av nm={s.nm} sz={22}/><span style={{fontSize:11,fontWeight:500}}>{s.nm}</span></div></button>)}</div></div>
      {parentNotes.length>0&&<div style={{...cd(),padding:12,marginBottom:12}}><h3 style={{fontFamily:FD,fontSize:13,margin:"0 0 6px"}}>Notas enviadas</h3>{parentNotes.sort((a,b)=>b.dt.localeCompare(a.dt)).map(n=>{const st=(D.sts||[]).find(x=>x.id===n.sid);const co=n.type==="derivacion"?"#DC2626":n.type==="seguimiento"?"#D97706":n.type==="reunion"?"#3B82F6":n.type==="felicitacion"?"#059669":P;return <div key={n.id} style={{padding:6,borderLeft:"3px solid "+co,background:co+"08",borderRadius:8,marginBottom:4}}><div style={{display:"flex",justifyContent:"space-between"}}><span style={{fontSize:11,fontWeight:600}}>{st?.nm||"—"}</span><div style={{display:"flex",gap:3}}><Pill t={n.type||"info"} co={co} bg={co+"15"}/><span style={{fontSize:10,color:TL}}>{fmt(n.dt)}</span></div></div><div style={{fontSize:12,marginTop:2}}>{n.text}</div></div>})}</div>}
      {myMsgs.length===0?<div style={{...cd(),padding:30,textAlign:"center"}}><MessageSquare size={30} color={BD}/><p style={{color:TL}}>Sin mensajes</p></div>:<div style={{...cd(),padding:12}}><h3 style={{fontFamily:FD,fontSize:13,margin:"0 0 6px"}}>Mensajes</h3>{myMsgs.sort((a,b)=>b.dt.localeCompare(a.dt)).map(m=>{const st=(D.sts||[]).find(x=>x.id===m.sid);const isMe=m.from===auth.uid;return <div key={m.id} style={{...cd({border:"none",boxShadow:"none"}),padding:8,marginBottom:3,borderLeft:isMe?"3px solid "+P:"3px solid #8B5CF6",background:BG}}><div style={{display:"flex",justifyContent:"space-between",marginBottom:2}}><span style={{fontSize:11,fontWeight:600,color:isMe?PD:"#8B5CF6"}}>{isMe?"Tú":"Familia"}{st?" → "+st.nm:""}</span><span style={{fontSize:10,color:TL}}>{fmt(m.dt)}</span></div><div style={{fontSize:12,color:TX}}>{m.text}</div></div>})}</div>}</div>;}
    return null;
  };

  return (
    <div style={{display:"flex",minHeight:"100vh",background:BG,fontFamily:F}}>
      <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;500;600;700;800&family=DM+Serif+Display&display=swap" rel="stylesheet"/>
      <SNav items={navs} active={v} onNav={k=>{setV(k);setSel(null);setStTab("overview");}} sub={D.school?.name||""} userName={user.nm||"Docente"} onLogout={logout}/>
      <div style={{flex:1,padding:18,overflowY:"auto",maxWidth:880,margin:"0 auto"}}>{content()}</div>
      {mdl==="addCls" && <AddClsModal authUid={auth.uid} D={D} up={up} onClose={()=>setMdl(null)}/>}
      {mdl==="addSt" && <AddStModal clsId={ac} D={D} up={up} onClose={()=>setMdl(null)}/>}
      {mdl==="journal" && <JournalModal D={D} up={up} onClose={()=>setMdl(null)}/>}
      {mdl==="dailyAn" && <DailyAnModal D={D} up={up} sts={sts} onClose={()=>setMdl(null)}/>}
      {mdl?.t==="eval" && <EvalModal student={mdl.s} D={D} up={up} onClose={()=>setMdl(null)}/>}
      {mdl?.t==="obs" && <ObsModal student={mdl.s} D={D} up={up} onClose={()=>setMdl(null)}/>}
      {mdl?.t==="pnote" && <PNoteModal student={mdl.s} D={D} up={up} authUid={auth.uid} onClose={()=>setMdl(null)}/>}
      {mdl?.t==="actD" && <ActDetailModal act={mdl.a} onClose={()=>setMdl(null)}/>}
      {mdl?.t==="assign" && <AssignActModal student={mdl.s} D={D} up={up} onClose={()=>setMdl(null)}/>}
      {mdl?.t==="pmi" && <PMIModal student={mdl.s} D={D} up={up} onClose={()=>setMdl(null)}/>}
      {mdl?.t==="mile" && <MilestoneModal student={mdl.s} D={D} up={up} onClose={()=>setMdl(null)}/>}
      {mdl?.t==="inc" && <IncidentModal student={mdl.s} D={D} up={up} onClose={()=>setMdl(null)}/>}
      {mdl?.t==="invite" && <InviteParentModal student={mdl.s} D={D} up={up} authUid={auth.uid} onClose={()=>setMdl(null)}/>}
      {mdl?.t==="guided" && <GuidedEvalModal student={mdl.s} D={D} up={up} onClose={()=>setMdl(null)}/>}
      {mdl==="createAct" && <CreateActModal D={D} up={up} onClose={()=>setMdl(null)}/>}
    </div>
  );
}

/* ═══ PARENT ═══ */
function ParentApp({D,up,logout}){
  const auth=D.auth||{};const par=(D.parents||[]).find(p=>p.id===auth.parId);
  const [v,setV]=useState("home");const [pdfMode,setPdfMode]=useState(false);
  const [linkCode,setLinkCode]=useState("");
  const [linkErr,setLinkErr]=useState("");
  if(!par)return <div style={{padding:40,textAlign:"center",fontFamily:F}}><p>Cuenta no encontrada</p><button onClick={logout} style={bp()}>Volver</button></div>;
  const chId=par.ch?.[0]||"";const ch=(D.sts||[]).find(s=>s.id===chId);
  const evs=(D.evs||[]).filter(e=>e.sid===chId);const lat=evs.length?evs.sort((a,b)=>b.dt.localeCompare(a.dt))[0]:null;
  const pctl=ch?getPctl(ch.dob):PCTL[4];
  const myMsgs=(D.msgs||[]).filter(m=>m.to===auth.parId||m.from===auth.parId);
  const myNotes=(D.pNotes||[]).filter(n=>par.ch?.includes(n.sid));
  const myInvs=(D.invitations||[]).filter(i=>i.parId===auth.parId);
  const pendInvs=(D.invitations||[]).filter(i=>i.parId===null&&i.status==="pending");
  const teacher=ch?(D.users||[]).find(u=>{const cls=(D.cls||[]).find(c=>c.id===ch.cid);return cls&&u.id===cls.tid;}):null;
  const doLink=()=>{setLinkErr("");if(!linkCode.trim()){setLinkErr("Introduce un código");return;}const newInvs=[...(D.invitations||[])];const inv=newInvs.find(i=>i.code===linkCode.trim().toUpperCase()&&i.status==="pending"&&!i.parId);if(!inv){setLinkErr("Código no válido o ya usado");return;}inv.parId=auth.parId;inv.status="accepted";const newPar=(D.parents||[]).map(p=>p.id===auth.parId?{...p,ch:[...(p.ch||[]),inv.sid]}:p);up({invitations:newInvs,parents:newPar});setLinkCode("");};
  const tabs=[{k:"home",l:"Inicio",I:Home},{k:"progress",l:"Progreso",I:TrendingUp},{k:"notes",l:"Notas",I:FileText},{k:"msgs",l:"Mensajes",I:MessageSquare},{k:"tips",l:"Recursos",I:Lightbulb}];
  return (
    <div style={{minHeight:"100vh",background:BG,fontFamily:F,maxWidth:500,margin:"0 auto"}}>
      <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;500;600;700;800&family=DM+Serif+Display&display=swap" rel="stylesheet"/>
      <div style={{background:"#fff",borderBottom:"1px solid "+BD,padding:"10px 14px",display:"flex",justifyContent:"space-between",alignItems:"center"}}><div style={{display:"flex",alignItems:"center",gap:6}}><NLogo sz={24}/><span style={{fontWeight:800,fontSize:12}}>NEXUS KIDS</span></div><button onClick={logout} style={{background:"none",border:"none",fontSize:11,color:TL,cursor:"pointer"}}>Salir</button></div>
      <div style={{padding:"12px 12px 70px"}}>
        {v==="home" && <div><h2 style={{fontFamily:FD,fontSize:19,margin:"0 0 12px"}}>Hola, {(par.nm||"").split(" ")[0]}</h2>
          {par.ch&&par.ch.length>1&&<div style={{display:"flex",gap:5,marginBottom:10,overflowX:"auto"}}>{par.ch.map(cid=>{const kid=(D.sts||[]).find(s=>s.id===cid);if(!kid)return null;return <button key={cid} onClick={()=>{}} style={{...bo(cid===chId,true),display:"flex",alignItems:"center",gap:4,whiteSpace:"nowrap"}}><Av nm={kid.nm} sz={18}/><span style={{fontSize:11}}>{kid.nm.split(" ")[0]}</span></button>})}</div>}
          {!ch?<div style={{...cd(),padding:20,textAlign:"center"}}><div style={{width:52,height:52,borderRadius:99,background:P+"15",display:"flex",alignItems:"center",justifyContent:"center",margin:"0 auto 12px"}}><UserPlus size={24} color={P}/></div>
          <h3 style={{fontFamily:FD,fontSize:15,margin:"0 0 6px"}}>Vincula a tu hijo/a</h3>
          <p style={{fontSize:12,color:TM,margin:"0 0 10px"}}>Introduce el código que te ha proporcionado el colegio para vincular tu cuenta con el perfil de tu hijo/a.</p>
          {D.school?.name&&<div style={{padding:6,background:PL,borderRadius:8,marginBottom:10,display:"inline-block"}}><span style={{fontSize:11,color:PD,fontWeight:600}}>{D.school.name}</span></div>}
          <input value={linkCode} onChange={e=>setLinkCode(e.target.value.toUpperCase())} placeholder="Ej: FER-2025" style={{...inp,textAlign:"center",fontSize:16,fontWeight:700,letterSpacing:3,fontFamily:"monospace",maxWidth:220,margin:"0 auto 6px",background:"#FFFBEB",border:"1.5px solid #F59E0B"}}/>
          {(()=>{const inv=(D.invitations||[]).find(i=>i.code===linkCode.trim().toUpperCase()&&i.status==="pending"&&!i.parId);if(!linkCode.trim()||!inv)return null;const st=(D.sts||[]).find(s=>s.id===inv.sid);const tea=(D.users||[]).find(u=>u.id===inv.tid);return <div style={{padding:10,background:PL,borderRadius:10,margin:"6px auto 6px",maxWidth:260,border:"1.5px solid "+P,textAlign:"left"}}><div style={{display:"flex",alignItems:"center",gap:8}}><Av nm={st?.nm||"?"} sz={32}/><div><div style={{fontSize:12,fontWeight:700,color:PD}}>✓ {st?.nm||"Alumno/a"}</div>{tea&&<div style={{fontSize:10,color:TM}}>Docente: {tea.nm}</div>}<div style={{fontSize:10,color:TM}}>{st?ageFn(st.dob):""}</div></div></div></div>})()}
          {linkErr&&<div style={{fontSize:11,color:"#DC2626",marginBottom:6}}>{linkErr}</div>}
          <button onClick={doLink} style={{...bp(),marginBottom:10}}>Vincular</button>
        </div>:lat?<div>{teacher&&<div style={{...cd(),padding:10,marginBottom:10,display:"flex",alignItems:"center",gap:8,background:PL}}><Av nm={teacher.nm} sz={30}/><div><div style={{fontSize:11,color:TM}}>Docente de {ch.nm}</div><div style={{fontSize:13,fontWeight:600,color:PD}}>{teacher.nm}</div></div></div>}<div style={{...cd(),padding:14,marginBottom:12}}><div style={{fontFamily:FD,fontSize:24,color:P}}>{gs(lat.sc)}%</div><div style={{fontSize:11,color:TM,marginBottom:8}}>Score global de {ch.nm} · {ageFn(ch.dob)}</div><Bars sc={lat.sc} sm={true} pctl={pctl}/></div><div style={{display:"grid",gridTemplateColumns:"1fr 1fr",gap:6}}>{DOM.map(d=>{const val=lat.sc[d.k]||0;const Ic=d.I;const sev=sevLb(val,pctl);return <div key={d.k} style={{...cd(),padding:8,background:sevBg(sev)}}><div style={{display:"flex",alignItems:"center",gap:3}}><Ic size={12} color={d.c}/><span style={{fontSize:11,fontWeight:600}}>{d.l}</span></div><div style={{fontSize:11,fontWeight:700,color:sevCo(sev),marginTop:2}}>{percLb(val)} ({val}%)</div></div>})}</div><button onClick={()=>setPdfMode(true)} style={{...bp(),width:"100%",marginTop:10,display:"flex",alignItems:"center",justifyContent:"center",gap:6,padding:"10px 16px",fontSize:12,fontWeight:600}}><FileText size={14}/>Descargar Informe PDF</button></div>:<div style={{...cd(),padding:30,textAlign:"center"}}>{teacher&&<div style={{marginBottom:10}}><Av nm={teacher.nm} sz={36}/><div style={{fontSize:12,fontWeight:600,color:PD,marginTop:4}}>{teacher.nm}</div><div style={{fontSize:11,color:TM}}>Docente de {ch.nm}</div></div>}<Clock size={30} color={BD}/><p style={{color:TL}}>Aún sin evaluar</p></div>}</div>}
        {v==="progress" && <div><h2 style={{fontFamily:FD,fontSize:19,margin:"0 0 12px"}}>Progreso</h2>{lat?<div>{DOM.map(d=>{const val=lat.sc[d.k]||0;const Ic=d.I;const sev=sevLb(val,pctl);return <div key={d.k} style={{...cd(),padding:10,marginBottom:6}}><div style={{display:"flex",justifyContent:"space-between",alignItems:"center"}}><span style={{display:"flex",alignItems:"center",gap:4}}><Ic size={14} color={d.c}/><span style={{fontSize:13,fontWeight:600}}>{d.l}</span></span><span style={{fontWeight:700,color:sevCo(sev)}}>{val}%</span></div><div style={{height:5,borderRadius:99,background:BL,marginTop:4,position:"relative"}}><div style={{height:"100%",borderRadius:99,background:d.c,width:val+"%"}}/><div style={{position:"absolute",top:-1,left:pctl.P50+"%",width:2,height:7,background:TM,borderRadius:1}}/></div><div style={{display:"flex",justifyContent:"space-between",marginTop:3,fontSize:10,color:TL}}><span>{sev==="danger"?"Por debajo del P10":sev==="warning"?"Por debajo del P25":sev==="excellent"?"Por encima del P90":"Dentro de lo esperado"}</span><span>P50: {pctl.P50}%</span></div></div>})}<button onClick={()=>setPdfMode(true)} style={{...bp(),width:"100%",marginTop:10,display:"flex",alignItems:"center",justifyContent:"center",gap:6,padding:"10px 16px",fontSize:12,fontWeight:600}}><FileText size={14}/>Descargar Informe PDF</button></div>:<div style={{...cd(),padding:30,textAlign:"center"}}><p style={{color:TL}}>Sin datos</p></div>}</div>}
        {v==="notes" && <div><h2 style={{fontFamily:FD,fontSize:19,margin:"0 0 12px"}}>{"Notas del centro"}</h2>{myNotes.length===0?<div style={{...cd(),padding:30,textAlign:"center"}}><p style={{color:TL}}>Sin notas</p></div>:myNotes.sort((a,b)=>b.dt.localeCompare(a.dt)).map(n=>{const co=n.type==="derivacion"?"#DC2626":n.type==="seguimiento"?"#D97706":n.type==="felicitacion"?"#059669":P;const st=(D.sts||[]).find(x=>x.id===n.sid);return <div key={n.id} style={{...cd(),padding:10,marginBottom:5,borderLeft:"3px solid "+co}}><div style={{display:"flex",justifyContent:"space-between"}}><span style={{fontSize:11,fontWeight:600}}>{st?.nm||""}</span><div style={{display:"flex",gap:3}}><Pill t={n.type||"info"} co={co} bg={co+"15"}/><span style={{fontSize:10,color:TL}}>{fmt(n.dt)}</span></div></div><div style={{fontSize:12,marginTop:3}}>{n.text}</div></div>})}</div>}
        {v==="msgs" && <div><h2 style={{fontFamily:FD,fontSize:19,margin:"0 0 12px"}}>Mensajes</h2>{myMsgs.length===0?<div style={{...cd(),padding:30,textAlign:"center"}}><p style={{color:TL}}>Sin mensajes</p></div>:myMsgs.sort((a,b)=>b.dt.localeCompare(a.dt)).map(m=>{const isMe=m.from===auth.parId;return <div key={m.id} style={{...cd(),padding:10,marginBottom:5,borderLeft:isMe?"3px solid #8B5CF6":"3px solid "+P}}><div style={{display:"flex",justifyContent:"space-between"}}><span style={{fontSize:11,fontWeight:600,color:isMe?"#8B5CF6":PD}}>{isMe?"Tú":"Docente"}</span><span style={{fontSize:10,color:TL}}>{fmt(m.dt)}</span></div><div style={{fontSize:12,marginTop:2}}>{m.text}</div></div>})}</div>}
        {v==="tips" && <div><h2 style={{fontFamily:FD,fontSize:19,margin:"0 0 12px"}}>Recursos para casa</h2>{[{dom:"soc",title:"Juegos en grupo",text:"Organiza quedadas con 1-2 niños. Juegos cooperativos como construir juntos."},{dom:"emo",title:"Emociones con cuentos",text:"Lee cuentos donde los personajes expresan emociones. Pregunta cómo se sienten."},{dom:"mot",title:"Motricidad en casa",text:"Plastilina, tijeras seguras, cuentas, dibujar. Fortalece manos y dedos."},{dom:"cog",title:"Juegos de memoria",text:"Memory, patrones rítmicos, puzzles progresivos adaptados a su edad."},{dom:"sen",title:"Exploración sensorial",text:"Texturas, sonidos, sabores nuevos. Integración multisensorial con juego."},{dom:"mtv",title:"Fomentar curiosidad",text:"Dejar que explore temas que le interesen. No dirigir, acompañar."}].map((t,i)=>{const d=DOM.find(x=>x.k===t.dom);const Ic=d?.I;return <div key={i} style={{...cd(),padding:12,marginBottom:6}}><div style={{display:"flex",alignItems:"center",gap:4,marginBottom:3}}>{Ic&&<Ic size={13} color={d.c}/>}<span style={{fontSize:13,fontWeight:600,color:d?.c}}>{t.title}</span></div><div style={{fontSize:12,color:TM}}>{t.text}</div></div>})}</div>}
      </div>
      {pdfMode&&ch&&lat&&(()=>{
        const g2r=gs(lat.sc);const ageR2=getAgeRange(ch.dob);const now=new Date();
        const per=(now.getDate()<=15?"1-15 ":"16-"+new Date(now.getFullYear(),now.getMonth()+1,0).getDate()+" ")+now.toLocaleDateString("es-ES",{month:"long"});
        const gC=g2r>=75?"#059669":g2r>=50?"#0D9488":g2r>=25?"#D97706":"#DC2626";
        const dC={cog:"#0D9488",emo:"#EC4899",soc:"#3B82F6",mot:"#F59E0B",sen:"#8B5CF6",mtv:"#059669"};
        const dN={cog:"Cognitivo",emo:"Emocional",soc:"Social",mot:"Motor",sen:"Sensorial",mtv:"Motivacional"};
        const dE={cog:"\u{1F9E0}",emo:"\u{1F497}",soc:"\u{1F465}",mot:"\u{26A1}",sen:"\u{1F441}",mtv:"\u{1F4A1}"};
        const pLx=(val,dk)=>{const dp=getDomPctl(dk,ch.dob);if(val<dp.P10)return["Necesita apoyo","#DC2626","#FEF2F2"];if(val<dp.P25)return["En desarrollo","#D97706","#FFFBEB"];if(val>dp.P90)return["Excelente","#059669","#ECFDF5"];return["Adecuado","#0D9488","#F0FDFA"];};
        const hSt={fontFamily:FD,fontSize:15,color:P,margin:"16px 0 8px",paddingBottom:4,borderBottom:"2px solid #E2E8F0"};
        const allEvChild=(D.evs||[]).filter(e=>e.sid===ch.id).sort((a,b)=>a.dt.localeCompare(b.dt));
        const prevEv=allEvChild.length>1?allEvChild[allEvChild.length-2]:null;
        const rAls=(D.als||[]).filter(a=>a.sid===ch.id&&a.st==="active");
        const rMil=(D.milestones||[]).filter(m=>m.sid===ch.id);
        const rNotes=(D.pNotes||[]).filter(n=>n.sid===ch.id).slice(-3);
        const pRx=PMI_RECS||{};
        return <div style={{position:"fixed",inset:0,zIndex:9999,background:"#fff",display:"flex",flexDirection:"column"}}>
          <div style={{display:"flex",alignItems:"center",justifyContent:"space-between",padding:"8px 16px",background:DARK,color:"#fff",flexShrink:0}}>
            <div style={{display:"flex",alignItems:"center",gap:8}}><div style={{width:28,height:28,borderRadius:99,background:P,display:"flex",alignItems:"center",justifyContent:"center",fontFamily:FD,fontWeight:700,fontSize:14,color:"#fff"}}>N</div><span style={{fontFamily:FD,fontSize:14}}>Informe de {ch.nm}</span></div>
            <div style={{display:"flex",gap:6}}>
              <button onClick={()=>window.print()} style={{...bp(),padding:"6px 14px",fontSize:11,display:"flex",alignItems:"center",gap:4}}><Printer size={13}/>Imprimir / Guardar PDF</button>
              <button onClick={()=>setPdfMode(false)} style={{background:"rgba(255,255,255,.15)",color:"#fff",border:"none",borderRadius:8,padding:"6px 12px",cursor:"pointer",fontSize:11,display:"flex",alignItems:"center",gap:4}}><X size={13}/>Cerrar</button>
            </div>
          </div>
          <div style={{flex:1,overflow:"auto",background:"#fff",padding:"0 16px 40px",maxWidth:850,margin:"0 auto",width:"100%",fontSize:11,lineHeight:1.6}}>
            <div style={{background:"linear-gradient(135deg,#0C3C36,#134E4A)",color:"#fff",padding:"20px 24px",borderRadius:"0 0 12px 12px",marginBottom:16}}>
              <div style={{float:"right",width:42,height:42,borderRadius:"50%",background:P,display:"flex",alignItems:"center",justifyContent:"center",fontFamily:FD,fontWeight:700,fontSize:22,color:"#fff"}}>N</div>
              <h1 style={{fontFamily:FD,fontSize:21,color:"#fff",margin:"0 0 2px"}}>Informe de Desarrollo</h1>
              <div style={{color:"#A7F3D0",fontSize:12}}>NEXUS KIDS — Informe para Familias</div>
              <div style={{display:"flex",gap:14,marginTop:8,fontSize:10,color:"#D1FAE5",flexWrap:"wrap"}}>
                <span>Alumno/a: <b>{ch.nm}</b></span>
                <span>Edad: <b>{ageFn(ch.dob)}</b> ({ageR2} años)</span>
                <span>Periodo: <b>{per+" "+now.getFullYear()}</b></span>
                <span>Fecha: <b>{now.toLocaleDateString("es-ES")}</b></span>
                {teacher&&<span>Docente: <b>{teacher.nm}</b></span>}
              </div>
            </div>
            <div style={{background:"#F0FDFA",border:"1px solid #E2E8F0",borderRadius:10,padding:14,marginBottom:12}}>
              <div style={{fontSize:12,color:TM,marginBottom:4}}>Querida familia,</div>
              <div style={{fontSize:11,color:TX,lineHeight:1.7}}>Este informe resume el desarrollo de <b>{ch.nm.split(" ")[0]}</b> durante este periodo. Los resultados reflejan observaciones realizadas en el aula y están diseñados para ayudarles a entender cómo evoluciona en las distintas áreas del desarrollo. Recuerden que cada niño/a tiene su propio ritmo.</div>
            </div>
            <h2 style={hSt}>Perfil Global</h2>
            <div style={{textAlign:"center",padding:8}}>
              <span style={{fontFamily:FD,fontSize:40,fontWeight:"bold",color:gC}}>{g2r}%</span>
              <div style={{fontSize:12,color:"#64748B"}}>{g2r>=75?"Desarrollo excelente":g2r>=50?"Desarrollo adecuado":g2r>=25?"Requiere atención":"Necesita apoyo profesional"}</div>
              {prevEv&&<div style={{fontSize:10,color:gs(lat.sc)>=gs(prevEv.sc)?"#059669":"#DC2626",marginTop:4}}>{gs(lat.sc)>=gs(prevEv.sc)?"\u{2191}":"↓"} {Math.abs(gs(lat.sc)-gs(prevEv.sc))}% vs evaluación anterior</div>}
            </div>
            <div style={{display:"grid",gridTemplateColumns:"1fr 1fr 1fr",gap:8,margin:"12px 0"}}>
              {DOM.map(d=>{const val=lat.sc[d.k]||0;const pl=pLx(val,d.k);const prev2=prevEv?(prevEv.sc[d.k]||0):null;const diff=prev2!==null?val-prev2:null;
                return <div key={d.k} style={{borderRadius:8,padding:10,textAlign:"center",border:"1px solid #E2E8F0",background:"#fff",borderTop:"3px solid "+dC[d.k]}}>
                  <div style={{fontSize:14}}>{dE[d.k]}</div>
                  <div style={{fontSize:12,fontWeight:600,color:dC[d.k]}}>{dN[d.k]}</div>
                  <div style={{fontFamily:FD,fontSize:26,fontWeight:"bold",color:pl[1]}}>{val}%</div>
                  <div style={{height:5,background:"#E2E8F0",borderRadius:99,margin:"5px 0"}}><div style={{height:"100%",borderRadius:99,width:val+"%",background:dC[d.k]}}/></div>
                  <span style={{display:"inline-block",padding:"2px 7px",borderRadius:99,fontSize:9,fontWeight:600,background:pl[2],color:pl[1]}}>{pl[0]}</span>
                  {diff!==null&&<div style={{fontSize:10,marginTop:3,color:diff>=0?"#059669":"#DC2626"}}>{(diff>=0?"+":"")+diff}% vs anterior</div>}
                </div>;})}
            </div>
            <h2 style={hSt}>Detalle por Área</h2>
            {DOM.map(d=>{const val=lat.sc[d.k]||0;const dp=getDomPctl(d.k,ch.dob);const pl=pLx(val,d.k);
              const bdr=val<dp.P10?"#DC2626":val<dp.P25?"#D97706":val>dp.P90?"#059669":"#0D9488";
              const bgc=val<dp.P10?"#FEF2F2":val<dp.P25?"#FFFBEB":val>dp.P90?"#ECFDF5":"#F0FDFA";
              const recs=((pRx[d.k]||{})[ageR2])||[];
              const parentTips={cog:["Juegos de memoria y puzzles adaptados a su edad","Lectura conjunta con preguntas sobre la historia","Juegos de clasificación y patrones"],emo:["Nombrar emociones cuando las sientan juntos","Cuentos sobre personajes que expresan sentimientos","Espacio seguro para expresar frustración"],soc:["Organizar quedadas con 1-2 niños","Juegos cooperativos en familia","Practicar turnos y compartir en casa"],mot:["Plastilina, tijeras seguras, ensartar cuentas","Juegos de equilibrio y circuitos motores","Pintar, dibujar y colorear a diario"],sen:["Explorar texturas, sonidos y sabores nuevos","Juegos con agua, arena, masas sensoriales","Paseos por la naturaleza con observación"],mtv:["Celebrar esfuerzos, no solo resultados","Dejar elegir actividades que le interesen","Proponer retos alcanzables con recompensa"]};
              return <div key={d.k} style={{background:bgc,border:"1px solid #E2E8F0",borderLeft:"4px solid "+bdr,borderRadius:8,padding:10,margin:"6px 0"}}>
                <div style={{display:"flex",justifyContent:"space-between",alignItems:"center"}}>
                  <div>{dE[d.k]+" "}<b style={{color:dC[d.k],fontSize:13}}>{dN[d.k]}</b></div>
                  <div><span style={{fontFamily:FD,fontSize:18,fontWeight:"bold",color:pl[1]}}>{val}%</span><span style={{display:"inline-block",padding:"2px 7px",borderRadius:99,fontSize:9,fontWeight:600,background:pl[2],color:pl[1],marginLeft:4}}>{pl[0]}</span></div>
                </div>
                <div style={{fontSize:10,color:"#64748B",margin:"5px 0"}}>Rango esperado para {ageR2} años: P25={dp.P25}% — P75={dp.P75||75}%</div>
                {val<dp.P50&&<div style={{marginTop:7}}>
                  <b style={{fontSize:10,color:"#0D9488"}}>Lo que pueden hacer en casa:</b>
                  {(parentTips[d.k]||recs).slice(0,3).map((r,ri)=><div key={ri} style={{padding:"6px 9px",background:"#fff",borderLeft:"3px solid "+dC[d.k],margin:"3px 0",borderRadius:"0 6px 6px 0",fontSize:10}}>{"\u{2192} "+r}</div>)}
                </div>}
                {val>=dp.P50&&<div style={{padding:"6px 9px",background:"#ECFDF5",borderLeft:"3px solid #059669",margin:"5px 0",borderRadius:"0 6px 6px 0",fontSize:10}}>{"\u{2705} Desarrollo adecuado o superior. \u{00A1}Sigan así!"}</div>}
              </div>;})}
            {allEvChild.length>1&&<div>
              <h2 style={hSt}>Evolución en el Tiempo</h2>
              <table style={{width:"100%",borderCollapse:"collapse",fontSize:10}}>
                <thead><tr><th style={{background:"#F1F5F9",padding:5,textAlign:"left",borderBottom:"2px solid #E2E8F0"}}>Fecha</th>
                  {DOM.map(d=><th key={d.k} style={{background:"#F1F5F9",padding:5,color:dC[d.k],borderBottom:"2px solid #E2E8F0"}}>{dN[d.k].slice(0,4)}</th>)}<th style={{background:"#F1F5F9",padding:5,borderBottom:"2px solid #E2E8F0"}}>Global</th></tr></thead>
                <tbody>{allEvChild.slice(-6).map((e2,ei)=><tr key={ei}><td style={{padding:4,borderBottom:"1px solid #F1F5F9"}}>{(e2.dt||"").slice(0,10)}</td>
                  {DOM.map(d=>{const v2=e2.sc[d.k]||0;const dp2=getDomPctl(d.k,ch.dob);return <td key={d.k} style={{padding:4,borderBottom:"1px solid #F1F5F9",color:v2<dp2.P10?"#DC2626":v2<dp2.P25?"#D97706":"#1E293B"}}>{v2}%</td>;})}<td style={{padding:4,borderBottom:"1px solid #F1F5F9",fontWeight:"bold"}}>{gs(e2.sc)}%</td></tr>)}</tbody>
              </table>
            </div>}
            {rAls.length>0&&<div><h2 style={hSt}>Áreas que Requieren Atención</h2>
              {rAls.map((a,ai)=>{const dk=a.dom||"cog";return <div key={ai} style={{background:a.sev==="high"?"#FEF2F2":"#FFFBEB",borderLeft:"4px solid "+(a.sev==="high"?"#DC2626":"#F59E0B"),borderRadius:8,padding:10,margin:"6px 0"}}><b>{dN[dk]||dk}</b>{" — "}{a.sev==="high"?"Se recomienda consulta profesional":"En seguimiento"}<div style={{fontSize:10,color:"#64748B"}}>{a.msg||""}</div></div>;})}</div>}
            {rMil.length>0&&<div><h2 style={hSt}>Logros Alcanzados</h2>
              <div style={{display:"grid",gridTemplateColumns:"1fr 1fr",gap:6}}>{rMil.map((m,mi)=>{const dk=m.dom||"cog";return <div key={mi} style={{background:"#F8FFFE",border:"1px solid #E2E8F0",borderRadius:8,padding:8}}>{(dE[dk]||"")+"\u{1F31F} "}<b>{m.text||""}</b><div style={{fontSize:9,color:"#94A3B8"}}>{(dN[dk]||"")+" — "+(m.dt||"").slice(0,10)}</div></div>;})}</div></div>}
            {rNotes.length>0&&<div><h2 style={hSt}>Notas del Centro</h2>
              {rNotes.map((n2,ni)=>{const co=n2.type==="derivacion"?"#DC2626":n2.type==="seguimiento"?"#D97706":n2.type==="felicitacion"?"#059669":P;return <div key={ni} style={{borderLeft:"3px solid "+co,padding:"6px 10px",marginBottom:4,borderRadius:"0 6px 6px 0",background:co+"08"}}><div style={{fontSize:10,fontWeight:600,color:co}}>{n2.type==="derivacion"?"Derivación":n2.type==="seguimiento"?"Seguimiento":n2.type==="felicitacion"?"Felicitación":"Información"}</div><div style={{fontSize:10,color:TX}}>{n2.text}</div></div>;})}</div>}
            <h2 style={hSt}>Recomendaciones para Casa</h2>
            <div style={{background:"#F0FDFA",border:"1px solid #E2E8F0",borderLeft:"4px solid #0D9488",borderRadius:8,padding:12,margin:"6px 0"}}>
              {(()=>{const weak=DOM.filter(d=>(lat.sc[d.k]||0)<(getDomPctl(d.k,ch.dob).P50));
                if(!weak.length)return <div style={{padding:6,textAlign:"center",color:"#059669"}}>Todos los dominios dentro de lo esperado. Sigan fomentando un ambiente rico en experiencias y afecto.</div>;
                return weak.map(d=>{const val=lat.sc[d.k]||0;const recs=((pRx[d.k]||{})[ageR2])||[];
                  return <div key={d.k} style={{margin:"8px 0",padding:10,background:"#fff",borderRadius:6,border:"1px solid #E2E8F0"}}>
                    <b style={{color:dC[d.k]}}>{dE[d.k]+" "+dN[d.k]+" — "+val+"%"}</b>
                    {recs.length>0&&<div style={{marginTop:5}}><b style={{fontSize:10}}>Objetivos ({ageR2} años):</b>{recs.slice(0,3).map((r,ri)=><div key={ri} style={{fontSize:10,margin:"2px 0 2px 8px"}}>{"\u{2192} "+r}</div>)}</div>}
                  </div>;});})()}
            </div>
            <div style={{textAlign:"center",fontSize:9,color:"#94A3B8",marginTop:20,paddingTop:10,borderTop:"1px solid #E2E8F0"}}>
              <div style={{marginBottom:3,fontSize:11,fontWeight:600,color:P}}>NEXUS KIDS</div>
              {"Informe generado el "+now.toLocaleDateString("",{day:"numeric",month:"long",year:"numeric"})+" — Documento confidencial."}<br/>
              {"Los resultados se basan en observaciones en el aula y no sustituyen una evaluación clínica profesional."}
            </div>
          </div>
        </div>;
      })()}
      <div style={{position:"fixed",bottom:0,left:0,right:0,maxWidth:500,margin:"0 auto",background:"#fff",borderTop:"1px solid "+BD,display:"flex",padding:"6px 0 10px",zIndex:50}}>{tabs.map(t=>{const Ic=t.I;return <button key={t.k} onClick={()=>setV(t.k)} style={{flex:1,border:"none",background:"none",cursor:"pointer",display:"flex",flexDirection:"column",alignItems:"center",gap:1,color:v===t.k?P:TL,fontFamily:F,fontSize:10,fontWeight:v===t.k?600:400}}><Ic size={16}/>{t.l}</button>})}</div>
    </div>
  );
}

/* ═══ ADMIN ═══ */
function AdminApp({D,up,logout}){
  const auth=D.auth||{};const user=(D.users||[]).find(u=>u.id===auth.uid)||{nm:"Super Admin",em:"admin",role:"admin"};
  const [v,setV]=useState("dash");const [mdl,setMdl]=useState(null);const [selU,setSelU]=useState(null);const [selSch,setSelSch]=useState(null);
  const [uSearch,setUSearch]=useState("");const [uFilter,setUFilter]=useState("all");const [auditF,setAuditF]=useState("all");
  const [editU,setEditU]=useState(null);const [confTab,setConfTab]=useState("general");
  const navs=[{k:"dash",l:"Dashboard",I:BarChart3},{k:"users",l:"Usuarios",I:Users},{k:"schools",l:"Colegios",I:School},{k:"pay",l:"Pagos",I:Activity},{k:"content",l:"Contenido",I:Layers},{k:"audit",l:"Auditoría",I:FileText},{k:"flags",l:"Feature Flags",I:Zap},{k:"config",l:"Config",I:Settings}];
  const allU=[...(D.users||[]),...(D.parents||[])];
  const sts=D.sts||[];const evs=D.evs||[];const als=(D.als||[]).filter(a=>a.st==="active");
  const log=(action,detail,entity)=>{const nl={id:uid(),dt:new Date().toISOString(),actor:user.nm,action,entity:entity||"Sistema",detail,ip:"local"};up({_auditLogs:[nl,...(D._auditLogs||[])]});};
  const defSchools=[{id:"s1",nm:"Colegio Stella Maris",country:"ES",plan:"standard",status:"active",students:Math.max(sts.length,24),teachers:Math.max((D.users||[]).filter(u=>u.role==="teacher").length,3),since:"2025-09-01",mrr:Math.max(sts.length,24)*5.5,contact:"director@stellamaris.es"},{id:"s2",nm:"Escola Nova Barcelona",country:"ES",plan:"premium",students:42,teachers:4,status:"active",since:"2025-10-15",mrr:294,contact:"info@escolanova.cat"},{id:"s3",nm:"Colegio San José",country:"ES",plan:"basic",students:18,teachers:2,status:"trial",since:"2026-01-20",mrr:0,trialEnd:"2026-03-05",contact:"admin@sanjose.es"},{id:"s4",nm:"Liceo Europeo Madrid",country:"ES",plan:"premium",students:85,teachers:8,status:"active",since:"2025-06-01",mrr:595,contact:"dir@liceoeuropeo.es"},{id:"s5",nm:"Escola del Mar",country:"PT",plan:"standard",students:30,teachers:3,status:"active",since:"2025-11-10",mrr:165,contact:"geral@escoladomar.pt"}];
  const schools=D._adminSchools||defSchools;const setSchools=ns=>up({_adminSchools:ns});
  const totalSt=schools.reduce((a,s)=>a+s.students,0);const totalTe=schools.reduce((a,s)=>a+s.teachers,0);
  const mrr=Math.round(schools.reduce((a,s)=>a+s.mrr,0));const arr=mrr*12;
  const mrrH=[mrr*.3,mrr*.38,mrr*.48,mrr*.55,mrr*.65,mrr*.72,mrr*.78,mrr*.85,mrr*.9,mrr*.94,mrr*.97,mrr];
  const churnR=2.1;const ltv=Math.round(mrr/Math.max(1,schools.length)/churnR*100);
  const seedLogs=[{id:"a1",dt:"2026-02-25T14:30:00",actor:"Director Pérez",action:"user.create",entity:"Usuario",detail:"Creó docente Ana López",ip:"85.62.14.22"},{id:"a2",dt:"2026-02-25T12:15:00",actor:"Sistema",action:"eval.complete",entity:"Evaluación",detail:"Evaluación completada: Lucía → 72%",ip:"-"},{id:"a3",dt:"2026-02-24T18:45:00",actor:"Director Pérez",action:"alert.resolve",entity:"Alerta",detail:"Resolvió alerta: Pablo (Motor bajo P10)",ip:"85.62.14.22"},{id:"a4",dt:"2026-02-24T09:00:00",actor:"Sistema",action:"payment.success",entity:"Pago",detail:"Factura #INV-2026-018 pagada: €467.50",ip:"-"},{id:"a5",dt:"2026-02-23T16:20:00",actor:"María García",action:"report.generate",entity:"Informe",detail:"Generó PDF quincenal: 12 alumnos",ip:"92.178.45.11"},{id:"a6",dt:"2026-02-22T14:10:00",actor:"Dra. Marta Ruiz",action:"referral.create",entity:"Derivación",detail:"Derivación creada: Pablo Martín → Neurología",ip:"88.14.233.67"},{id:"a7",dt:"2026-02-22T08:00:00",actor:"Sistema",action:"backup.complete",entity:"Sistema",detail:"Backup diario completado: 2.4GB",ip:"-"}];
  const auditLogs=[...(D._auditLogs||[]),...seedLogs];
  const filtAudit=auditF==="all"?auditLogs:auditLogs.filter(a=>a.action.startsWith(auditF));
  const defInv=[{id:"inv1",school:"Liceo Europeo Madrid",amount:595,dt:"2026-02-01",status:"paid",plan:"Premium"},{id:"inv2",school:"Colegio Stella Maris",amount:132,dt:"2026-02-01",status:"paid",plan:"Standard"},{id:"inv3",school:"Escola Nova Barcelona",amount:294,dt:"2026-02-01",status:"paid",plan:"Premium"},{id:"inv4",school:"Escola del Mar",amount:165,dt:"2026-02-01",status:"paid",plan:"Standard"},{id:"inv5",school:"Colegio San José",amount:0,dt:"2026-01-20",status:"trial",plan:"Basic (Trial)"}];
  const invoices=D._invoices||defInv;const setInvoices=ni=>up({_invoices:ni});
  const defCoupons=[{id:"c1",code:"PILOT50",desc:"50% off 3 meses",pct:50,uses:3,max:10,on:true},{id:"c2",code:"EDTECH2026",desc:"20% off primer año",pct:20,uses:1,max:50,on:true},{id:"c3",code:"EARLYBIRD",desc:"30% off",pct:30,uses:8,max:8,on:false}];
  const coupons=D._coupons||defCoupons;const setCoupons=nc=>up({_coupons:nc});
  const defVers=[{id:"cv1",ver:"v1.0",dt:"2025-09-01",status:"published",author:"CTO",notes:"Versión inicial completa"},{id:"cv2",ver:"v0.9-beta",dt:"2025-07-15",status:"archived",author:"CTO",notes:"Beta con 80 ítems"},{id:"cv3",ver:"v0.8-alpha",dt:"2025-05-01",status:"archived",author:"Equipo científico",notes:"Alpha con 60 ítems"}];
  const versions=D._versions||defVers;const setVersions=nv=>up({_versions:nv});
  const defFlags={ai_predictive:false,parent_portal:true,screening_v2:true,max_trial_days:14,maintenance:false,eval_frequency:15,guided_obs:true,pdf_quincenal:true};
  const flags=D._featureFlags||defFlags;const setFlags=nf=>up({_featureFlags:nf});
  const toggleFlag=k=>{const nf={...flags,[k]:!flags[k]};setFlags(nf);log("system.flag_toggle","Flag '"+k+"' → "+(nf[k]?"ON":"OFF"),"Feature Flag");};
  const cfg=D._cfg||{schoolName:D.school?.name||"",country:"ES",email:"admin@nexuskids.com",tz:"Europe/Madrid",p10:10,p25:25,p90:90,scale:"likert04"};
  const setCfg=(k,val)=>up({_cfg:{...cfg,[k]:val}});
  const defEmails=[{id:"e1",l:"Bienvenida",desc:"Se envía al crear cuenta",on:true},{id:"e2",l:"Trial expirando",desc:"3 días antes del fin",on:true},{id:"e3",l:"Pago fallido",desc:"Cuando falla un cobro",on:true},{id:"e4",l:"Informe listo",desc:"Cuando se genera un PDF",on:false},{id:"e5",l:"Resumen semanal",desc:"Actividad para docentes",on:false}];
  const emails=D._emails||defEmails;const toggleEmail=id=>{const ne=emails.map(e=>e.id===id?{...e,on:!e.on}:e);up({_emails:ne});log("config.email","Email '"+emails.find(e=>e.id===id)?.l+"' toggle","Config");};
  const defLangs=[{code:"ES",name:"Español",on:true,keys:236},{code:"EN",name:"English",on:false,keys:0},{code:"PT",name:"Português",on:false,keys:0},{code:"FR",name:"Français",on:false,keys:0},{code:"IT",name:"Italiano",on:false,keys:0},{code:"DE",name:"Deutsch",on:false,keys:0}];
  const langs=D._langs||defLangs;const toggleLang=code=>{const nl=langs.map(l=>l.code===code?{...l,on:!l.on}:l);up({_langs:nl});log("config.lang","Idioma '"+code+"' toggle","Config");};
  /* helpers */
  const KPI=({label,value,sub,color,icon:Ic})=><div style={{...cd(),padding:14}}><div style={{display:"flex",justifyContent:"space-between",alignItems:"flex-start"}}><div><div style={{fontSize:10,fontWeight:600,color:TM,textTransform:"uppercase",letterSpacing:.5,marginBottom:4}}>{label}</div><div style={{fontFamily:FD,fontSize:24,color:TX,lineHeight:1}}>{value}</div>{sub&&<div style={{fontSize:10,color:color||P,fontWeight:600,marginTop:3}}>{sub}</div>}</div>{Ic&&<div style={{width:34,height:34,borderRadius:10,background:(color||P)+"15",display:"flex",alignItems:"center",justifyContent:"center"}}><Ic size={17} color={color||P}/></div>}</div></div>;
  const Bg=({text,color:bc})=><span style={{fontSize:10,fontWeight:700,padding:"3px 8px",borderRadius:6,background:(bc||P)+"15",color:bc||P}}>{text}</span>;
  const SH=({title,sub,right})=><div style={{display:"flex",justifyContent:"space-between",alignItems:"center",marginBottom:14}}><div><h2 style={{fontFamily:FD,fontSize:20,margin:0,color:TX}}>{title}</h2>{sub&&<p style={{fontSize:12,color:TM,margin:"2px 0 0"}}>{sub}</p>}</div>{right&&<div style={{display:"flex",gap:6,alignItems:"center"}}>{right}</div>}</div>;
  const filtU=allU.filter(u=>{const q=uSearch.toLowerCase();const match=!q||(u.nm||"").toLowerCase().includes(q)||(u.em||"").toLowerCase().includes(q);const filt=uFilter==="all"||u.role===uFilter||(uFilter==="parent"&&!u.role);return match&&filt;});
  const suspendUser=uid2=>{up({users:(D.users||[]).map(u=>u.id===uid2?{...u,suspended:!u.suspended}:u),parents:(D.parents||[]).map(p2=>p2.id===uid2?{...p2,suspended:!p2.suspended}:p2)});const t=allU.find(u=>u.id===uid2);log("user."+(t?.suspended?"reactivate":"suspend"),(t?.suspended?"Reactivó":"Suspendió")+": "+(t?.nm||""),"Usuario");};
  const deleteUser=uid2=>{const t=allU.find(u=>u.id===uid2);if(!confirm("¿Eliminar a "+(t?.nm||"")+"?"))return;up({users:(D.users||[]).filter(u=>u.id!==uid2),parents:(D.parents||[]).filter(p2=>p2.id!==uid2)});log("user.delete","Eliminó: "+(t?.nm||""),"Usuario");};
  const resetPin=uid2=>{up({users:(D.users||[]).map(u=>u.id===uid2?{...u,pin:"0000"}:u),parents:(D.parents||[]).map(p2=>p2.id===uid2?{...p2,pin:"0000"}:p2)});const t=allU.find(u=>u.id===uid2);log("user.reset_pin","Reset PIN: "+(t?.nm||"")+" → 0000","Usuario");alert("PIN reseteado a: 0000");};
  const getEvalCnt=uid2=>(evs||[]).filter(e=>e.evaluatorId===uid2).length||(uid2==="t1"?evs.length:0);
  const plCo=p2=>p2==="premium"?"#8B5CF6":p2==="standard"?P:"#64748B";
  const stCo=s=>s==="active"?"#059669":s==="trial"?"#F59E0B":s==="suspended"?"#DC2626":"#64748B";
  const plPr=p2=>p2==="premium"?7:p2==="standard"?5.5:3.75;
  const changePlan=(sid,np)=>{const ns=schools.map(s=>s.id===sid?{...s,plan:np,mrr:s.students*plPr(np)}:s);setSchools(ns);log("school.plan","Cambió plan: "+schools.find(s=>s.id===sid)?.nm+" → "+np,"Colegio");};
  const extendTrial=sid=>{const ns=schools.map(s=>{if(s.id!==sid)return s;const d=s.trialEnd?new Date(s.trialEnd):new Date();d.setDate(d.getDate()+14);return{...s,trialEnd:d.toISOString().slice(0,10)};});setSchools(ns);log("school.trial","Extendió trial +14d: "+schools.find(s=>s.id===sid)?.nm,"Colegio");alert("Trial extendido +14 días");};
  const suspendSchool=sid=>{const s=schools.find(x=>x.id===sid);const ns=schools.map(x=>x.id===sid?{...x,status:x.status==="suspended"?"active":"suspended",mrr:x.status==="suspended"?x.students*plPr(x.plan):0}:x);setSchools(ns);log("school."+(s?.status==="suspended"?"reactivate":"suspend"),(s?.status==="suspended"?"Reactivó":"Suspendió")+": "+s?.nm,"Colegio");};
  const exportSchool=sid=>{const s=schools.find(x=>x.id===sid);const csv="Nombre,Plan,Alumnos,Docentes,MRR,Estado\n"+s.nm+","+s.plan+","+s.students+","+s.teachers+","+Math.round(s.mrr)+","+s.status;const b=new Blob([csv],{type:"text/csv"});const u=URL.createObjectURL(b);const a=document.createElement("a");a.href=u;a.download=s.nm.replace(/\s/g,"_")+".csv";a.click();URL.revokeObjectURL(u);log("school.export","Exportó: "+s.nm,"Colegio");};
  const addSchool=d=>{setSchools([...schools,{id:uid(),nm:d.nm,country:d.country||"ES",plan:d.plan||"basic",status:"trial",students:parseInt(d.students)||0,teachers:parseInt(d.teachers)||0,since:td(),mrr:0,trialEnd:new Date(Date.now()+14*86400000).toISOString().slice(0,10),contact:d.contact||""}]);log("school.create","Alta: "+d.nm,"Colegio");};
  const addCoupon=d=>{setCoupons([...coupons,{id:uid(),code:d.code.toUpperCase(),desc:d.desc,pct:parseInt(d.pct)||0,uses:0,max:parseInt(d.max)||10,on:true}]);log("payment.coupon","Creó cupón: "+d.code,"Pago");};
  const toggleCoupon=id=>{const nc=coupons.map(c=>c.id===id?{...c,on:!c.on}:c);setCoupons(nc);log("payment.coupon_toggle","Cupón "+coupons.find(c=>c.id===id)?.code+" toggle","Pago");};
  const refundInv=id=>{const ni=invoices.map(i=>i.id===id?{...i,status:"refunded"}:i);setInvoices(ni);const inv=invoices.find(i=>i.id===id);log("payment.refund","Reembolso: "+inv?.school+" €"+inv?.amount,"Pago");};
  const createDraft=()=>{const last=versions[0];const m=last?.ver?.match(/(\d+)\.(\d+)/);const nv=m?"v"+m[1]+"."+(parseInt(m[2])+1):"v1.1";setVersions([{id:uid(),ver:nv,dt:td(),status:"draft",author:user.nm,notes:""},...versions]);log("content.draft","Creó draft: "+nv,"Contenido");};
  const publishVer=id=>{setVersions(versions.map(v2=>v2.id===id?{...v2,status:"published",dt:td()}:v2.status==="published"?{...v2,status:"archived"}:v2));log("content.publish","Publicó: "+versions.find(v2=>v2.id===id)?.ver,"Contenido");};
  const rollbackVer=id=>{setVersions(versions.map(v2=>v2.id===id?{...v2,status:"published",dt:td()}:v2.status==="published"?{...v2,status:"archived"}:v2));log("content.rollback","Rollback: "+versions.find(v2=>v2.id===id)?.ver,"Contenido");};
  const updateVerNotes=(id,n)=>setVersions(versions.map(v2=>v2.id===id?{...v2,notes:n}:v2));
  const exportAll=()=>{const d=JSON.stringify({schools,invoices,coupons,users:allU.length,evals:evs.length,alerts:als.length,mrr,arr,flags,cfg},null,2);const b=new Blob([d],{type:"application/json"});const u=URL.createObjectURL(b);const a=document.createElement("a");a.href=u;a.download="nexus_export_"+td()+".json";a.click();URL.revokeObjectURL(u);log("system.export","Exportación completa","Sistema");};
  const exportAudit=()=>{const csv="Fecha,Actor,Acción,Detalle,IP\n"+auditLogs.map(a=>'"'+a.dt+'","'+a.actor+'","'+a.action+'","'+a.detail+'","'+a.ip+'"').join("\n");const b=new Blob([csv],{type:"text/csv"});const u=URL.createObjectURL(b);const a=document.createElement("a");a.href=u;a.download="nexus_audit_"+td()+".csv";a.click();URL.revokeObjectURL(u);log("system.audit_export","Exportó auditoría","Sistema");};
  const Tog=({on,onClick,sz})=>{const w=sz||48;const h=Math.round(w*.54);const dot=Math.round(h*.77);const pad=Math.round((h-dot)/2);return <button onClick={onClick} style={{width:w,height:h,borderRadius:h/2,border:"none",background:on?"#059669":"#CBD5E1",cursor:"pointer",position:"relative",transition:"background .2s"}}><div style={{width:dot,height:dot,borderRadius:dot/2,background:"#fff",position:"absolute",top:pad,left:on?w-dot-pad:pad,transition:"left .2s",boxShadow:"0 1px 3px rgba(0,0,0,.2)"}}/></button>;};
  return (
    <div style={{display:"flex",minHeight:"100vh",background:BG,fontFamily:F}}>
      <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;500;600;700;800&family=DM+Serif+Display&display=swap" rel="stylesheet"/>
      <SNav items={navs} active={v} onNav={k=>{setV(k);setSelU(null);setSelSch(null);}} sub="Administración" userName={user.nm||""} onLogout={logout}/>
      <div style={{flex:1,padding:18,overflowY:"auto",maxWidth:960,margin:"0 auto"}}>
        {/* ═══ DASHBOARD ═══ */}
        {v==="dash"&&<div>
          <SH title="Panel de Administración" sub={"Actualizado: "+new Date().toLocaleTimeString("es-ES",{hour:"2-digit",minute:"2-digit"})} right={<><button onClick={exportAll} style={bo(false,true)}>⬇ Exportar</button><button onClick={()=>setV("audit")} style={bo(false,true)}>Auditoría</button></>}/>
          <div style={{display:"grid",gridTemplateColumns:"repeat(4,1fr)",gap:10,marginBottom:14}}>
            <KPI label="MRR" value={"€"+mrr.toLocaleString()} sub="+12% vs anterior" color="#059669" icon={TrendingUp}/>
            <KPI label="ARR" value={"€"+arr.toLocaleString()} color="#0EA5E9" icon={BarChart2}/>
            <KPI label="Churn" value={churnR+"%"} sub="↓ 0.3%" color="#059669" icon={Activity}/>
            <KPI label="LTV" value={"€"+ltv} color="#8B5CF6" icon={Star}/>
          </div>
          <div style={{display:"grid",gridTemplateColumns:"repeat(4,1fr)",gap:10,marginBottom:14}}>
            <KPI label="Colegios" value={schools.filter(s=>s.status==="active").length} sub={schools.filter(s=>s.status==="trial").length+" trial"} icon={School}/>
            <KPI label="Alumnos" value={totalSt} icon={Users}/>
            <KPI label="Docentes" value={totalTe} icon={GraduationCap}/>
            <KPI label="Trial→Paid" value="78%" color="#059669" icon={CheckCircle}/>
          </div>
          <div style={{...cd(),padding:16,marginBottom:14}}>
            <div style={{fontSize:12,fontWeight:700,color:TX,marginBottom:4}}>Evolución MRR (12 meses)</div>
            <div style={{display:"flex",alignItems:"flex-end",gap:3,height:100}}>
              {mrrH.map((val,i)=>{const mx=Math.max(...mrrH);return <div key={i} style={{flex:1,display:"flex",flexDirection:"column",alignItems:"center",gap:2}}><div style={{fontSize:8,color:TM}}>€{Math.round(val)}</div><div style={{width:"100%",background:i===11?"linear-gradient(180deg,#059669,#0D9488)":P+"35",borderRadius:3,height:Math.max(6,(val/mx)*90)}}/><div style={{fontSize:8,color:TL}}>{["Mar","Abr","May","Jun","Jul","Ago","Sep","Oct","Nov","Dic","Ene","Feb"][i]}</div></div>})}
            </div>
          </div>
          <div style={{display:"grid",gridTemplateColumns:"1fr 1fr",gap:12}}>
            <div style={{...cd(),padding:14}}>
              <div style={{fontSize:12,fontWeight:700,color:TX,marginBottom:10}}>Revenue por Plan</div>
              {[{pl:"Premium",co:"#8B5CF6"},{pl:"Standard",co:P},{pl:"Basic",co:"#64748B"}].map(x=>{const rev=schools.filter(s=>s.plan===x.pl.toLowerCase()).reduce((a,s)=>a+s.mrr,0);const pct=mrr>0?Math.round(rev/mrr*100):0;return <div key={x.pl} style={{marginBottom:8}}><div style={{display:"flex",justifyContent:"space-between",fontSize:11,marginBottom:3}}><span style={{fontWeight:600}}>{x.pl}</span><span style={{color:TM}}>€{Math.round(rev)} ({pct}%)</span></div><div style={{height:6,background:BL,borderRadius:3,overflow:"hidden"}}><div style={{height:"100%",width:Math.max(1,pct)+"%",background:x.co,borderRadius:3}}/></div></div>})}
            </div>
            <div style={{...cd(),padding:14}}>
              <div style={{fontSize:12,fontWeight:700,color:TX,marginBottom:10}}>Actividad Reciente</div>
              {auditLogs.slice(0,6).map(a=><div key={a.id} style={{display:"flex",gap:8,marginBottom:7,alignItems:"flex-start"}}><div style={{width:6,height:6,borderRadius:3,background:a.action.includes("payment")||a.action.includes("publish")?"#059669":a.action.includes("suspend")||a.action.includes("delete")?"#DC2626":P,marginTop:5,flexShrink:0}}/><div><div style={{fontSize:11,color:TX}}>{a.detail}</div><div style={{fontSize:9,color:TL}}>{a.actor} · {a.dt.slice(0,10)}</div></div></div>)}
            </div>
          </div>
        </div>}
        {/* ═══ USERS ═══ */}
        {v==="users"&&!selU&&<div>
          <SH title={"Usuarios ("+allU.length+")"} sub="Gestión completa" right={<><input value={uSearch} onChange={e=>setUSearch(e.target.value)} placeholder="Buscar..." style={{...inp,width:170,padding:"7px 12px",fontSize:12}}/><button onClick={()=>setMdl("addU")} style={bp(true)}>+ Nuevo</button></>}/>
          <div style={{display:"flex",gap:4,marginBottom:12,flexWrap:"wrap"}}>{[{k:"all",l:"Todos ("+allU.length+")"},{k:"teacher",l:"Docentes"},{k:"admin",l:"Admins"},{k:"psico",l:"Psico"},{k:"parent",l:"Familias"}].map(f=><button key={f.k} onClick={()=>setUFilter(f.k)} style={bo(uFilter===f.k,true)}>{f.l}</button>)}</div>
          <div style={{...cd(),overflow:"hidden"}}>
            <div style={{display:"grid",gridTemplateColumns:"2fr 2fr 1fr 1fr 1fr 100px",padding:"10px 14px",background:DARK,gap:8}}>{["Usuario","Email","Rol","Evals","Estado","Acciones"].map(h=><div key={h} style={{fontSize:10,fontWeight:700,color:"#fff",textTransform:"uppercase",letterSpacing:.5}}>{h}</div>)}</div>
            {filtU.length===0?<div style={{padding:30,textAlign:"center",color:TL}}>Sin resultados</div>:filtU.map((u,i)=><div key={u.id} style={{display:"grid",gridTemplateColumns:"2fr 2fr 1fr 1fr 1fr 100px",padding:"10px 14px",gap:8,borderBottom:"1px solid "+BL,background:i%2===0?"#fff":"#FAFBFC",cursor:"pointer",alignItems:"center"}} onClick={()=>setSelU(u)}>
              <div style={{display:"flex",alignItems:"center",gap:8}}><Av nm={u.nm} sz={26}/><div style={{fontSize:12,fontWeight:600}}>{u.nm}</div></div>
              <div style={{fontSize:11,color:TM}}>{u.em}</div>
              <div><Bg text={u.role==="teacher"?"Docente":u.role==="admin"?"Admin":u.role==="psico"?"Psico":"Familia"} color={u.role==="admin"?"#DC2626":u.role==="psico"?"#8B5CF6":u.role==="teacher"?P:"#F59E0B"}/></div>
              <div style={{fontSize:12,fontWeight:600}}>{getEvalCnt(u.id)}</div>
              <div><Bg text={u.suspended?"Suspendido":"Activo"} color={u.suspended?"#DC2626":"#059669"}/></div>
              <div style={{display:"flex",gap:3}} onClick={e=>e.stopPropagation()}>
                <button onClick={()=>{setEditU(u);setMdl("editU");}} style={{background:"none",border:"none",cursor:"pointer",padding:3}} title="Editar"><Edit3 size={13} color={TM}/></button>
                <button onClick={()=>suspendUser(u.id)} style={{background:"none",border:"none",cursor:"pointer",padding:3}} title="Suspender"><Shield size={13} color={u.suspended?"#059669":"#D97706"}/></button>
                <button onClick={()=>resetPin(u.id)} style={{background:"none",border:"none",cursor:"pointer",padding:3}} title="Reset PIN"><Clock size={13} color="#0EA5E9"/></button>
              </div>
            </div>)}
          </div>
        </div>}
        {v==="users"&&selU&&<div>
          <button onClick={()=>setSelU(null)} style={{...bo(false,true),marginBottom:12,display:"flex",alignItems:"center",gap:4}}><ChevronLeft size={14}/>Volver</button>
          <div style={{display:"grid",gridTemplateColumns:"1fr 2fr",gap:14}}>
            <div style={{...cd(),padding:16,textAlign:"center"}}>
              <Av nm={selU.nm} sz={64}/><h3 style={{fontFamily:FD,fontSize:16,margin:"10px 0 2px"}}>{selU.nm}</h3><div style={{fontSize:12,color:TM}}>{selU.em}</div>
              <div style={{marginTop:8}}><Bg text={selU.role==="teacher"?"Docente":selU.role==="admin"?"Admin":selU.role==="psico"?"Psico":"Familia"} color={selU.role==="admin"?"#DC2626":P}/></div>
              <div style={{marginTop:12,display:"flex",flexDirection:"column",gap:6}}>
                <button onClick={()=>{setEditU(selU);setMdl("editU");}} style={{...bo(false,true),width:"100%",fontSize:11}}>✏️ Editar</button>
                <button onClick={()=>resetPin(selU.id)} style={{...bo(false,true),width:"100%",fontSize:11}}>🔑 Reset PIN</button>
                <button onClick={()=>{suspendUser(selU.id);setSelU({...selU,suspended:!selU.suspended});}} style={{...bo(false,true),width:"100%",fontSize:11,color:selU.suspended?"#059669":"#D97706"}}>{selU.suspended?"✅ Reactivar":"⏸ Suspender"}</button>
                <button onClick={()=>{deleteUser(selU.id);setSelU(null);}} style={{...bo(false,true),width:"100%",fontSize:11,color:"#DC2626",borderColor:"#FCA5A5"}}>🗑 Eliminar</button>
              </div>
            </div>
            <div>
              <div style={{...cd(),padding:14,marginBottom:10}}><div style={{fontSize:12,fontWeight:700,marginBottom:8}}>Información</div><div style={{display:"grid",gridTemplateColumns:"1fr 1fr",gap:8}}>{[{l:"Rol",v:selU.role||"parent"},{l:"Evaluaciones",v:getEvalCnt(selU.id)},{l:"Estado",v:selU.suspended?"Suspendido":"Activo"},{l:"PIN",v:selU.pin||"****"}].map(f=><div key={f.l}><div style={{fontSize:10,color:TM}}>{f.l}</div><div style={{fontSize:13,fontWeight:600}}>{f.v}</div></div>)}</div></div>
              <div style={{...cd(),padding:14}}><div style={{fontSize:12,fontWeight:700,marginBottom:8}}>Actividad reciente</div>{auditLogs.filter(a=>a.actor===selU.nm||a.detail.includes(selU.nm)).length>0?auditLogs.filter(a=>a.actor===selU.nm||a.detail.includes(selU.nm)).slice(0,8).map(a=><div key={a.id} style={{fontSize:11,color:TM,marginBottom:4}}>{a.dt.slice(0,10)} — {a.detail}</div>):<div style={{fontSize:11,color:TL}}>Sin actividad</div>}</div>
            </div>
          </div>
        </div>}
        {/* ═══ SCHOOLS ═══ */}
        {v==="schools"&&!selSch&&<div>
          <SH title={"Colegios ("+schools.length+")"} sub="Centros educativos y licencias" right={<button onClick={()=>setMdl("addSch")} style={bp(true)}>+ Nuevo</button>}/>
          <div style={{display:"grid",gridTemplateColumns:"repeat(3,1fr)",gap:10,marginBottom:14}}>
            <KPI label="Activos" value={schools.filter(s=>s.status==="active").length} color="#059669" icon={CheckCircle}/>
            <KPI label="Trial" value={schools.filter(s=>s.status==="trial").length} color="#F59E0B" icon={Clock}/>
            <KPI label="MRR" value={"€"+mrr} color={P} icon={TrendingUp}/>
          </div>
          {schools.map(s=><div key={s.id} style={{...cd(),padding:14,marginBottom:8,cursor:"pointer"}} onClick={()=>setSelSch(s)}><div style={{display:"flex",justifyContent:"space-between",alignItems:"center"}}><div style={{display:"flex",alignItems:"center",gap:10}}><div style={{width:40,height:40,borderRadius:10,background:plCo(s.plan)+"15",display:"flex",alignItems:"center",justifyContent:"center"}}><School size={20} color={plCo(s.plan)}/></div><div><div style={{fontSize:14,fontWeight:700}}>{s.nm}</div><div style={{display:"flex",gap:8,marginTop:3}}><span style={{fontSize:11,color:TM}}>{s.students} alum.</span><span style={{fontSize:11,color:TM}}>{s.teachers} doc.</span><span style={{fontSize:11,color:TM}}>{s.country}</span></div></div></div><div style={{display:"flex",alignItems:"center",gap:8}}><Bg text={s.plan.charAt(0).toUpperCase()+s.plan.slice(1)} color={plCo(s.plan)}/><Bg text={s.status==="active"?"Activo":s.status==="trial"?"Trial":"Suspendido"} color={stCo(s.status)}/>{s.mrr>0&&<span style={{fontSize:13,fontWeight:700,color:"#059669"}}>€{Math.round(s.mrr)}/m</span>}</div></div></div>)}
        </div>}
        {v==="schools"&&selSch&&<div>
          <button onClick={()=>setSelSch(null)} style={{...bo(false,true),marginBottom:12,display:"flex",alignItems:"center",gap:4}}><ChevronLeft size={14}/>Volver</button>
          <div style={{...cd(),padding:18,marginBottom:14}}>
            <div style={{display:"flex",justifyContent:"space-between",alignItems:"center",marginBottom:14}}><div><h2 style={{fontFamily:FD,fontSize:20,margin:0}}>{selSch.nm}</h2><div style={{fontSize:12,color:TM,marginTop:2}}>Desde {fmt(selSch.since)} · {selSch.country} · {selSch.contact||""}</div></div><div style={{display:"flex",gap:6}}><Bg text={selSch.plan.charAt(0).toUpperCase()+selSch.plan.slice(1)} color={plCo(selSch.plan)}/><Bg text={selSch.status==="active"?"Activo":selSch.status==="trial"?"Trial":"Suspendido"} color={stCo(selSch.status)}/></div></div>
            <div style={{display:"grid",gridTemplateColumns:"repeat(4,1fr)",gap:10}}><KPI label="Alumnos" value={selSch.students} icon={Users}/><KPI label="Docentes" value={selSch.teachers} icon={GraduationCap}/><KPI label="MRR" value={"€"+Math.round(selSch.mrr)} color="#059669" icon={TrendingUp}/><KPI label="Evals" value={selSch.id==="s1"?evs.length:Math.round(selSch.students*2.3)} icon={Clipboard}/></div>
          </div>
          <div style={{display:"grid",gridTemplateColumns:"1fr 1fr",gap:12}}>
            <div style={{...cd(),padding:14}}><div style={{fontSize:12,fontWeight:700,marginBottom:8}}>Plan</div>{[{l:"Plan",v:selSch.plan},{l:"€/alumno/mes",v:"€"+plPr(selSch.plan).toFixed(2)},{l:"MRR",v:"€"+Math.round(selSch.mrr)},{l:"Trial hasta",v:selSch.trialEnd||"N/A"},{l:"Contacto",v:selSch.contact||"—"}].map(r=><div key={r.l} style={{display:"flex",justifyContent:"space-between",padding:"6px 0",borderBottom:"1px solid "+BL}}><span style={{fontSize:11,color:TM}}>{r.l}</span><span style={{fontSize:11,fontWeight:600}}>{r.v}</span></div>)}</div>
            <div style={{...cd(),padding:14}}><div style={{fontSize:12,fontWeight:700,marginBottom:8}}>Acciones</div><div style={{display:"flex",flexDirection:"column",gap:6}}>
              <div style={{fontSize:11,fontWeight:600,color:TM,marginBottom:2}}>Cambiar plan:</div>
              <div style={{display:"flex",gap:4}}>{["basic","standard","premium"].map(pl=><button key={pl} onClick={()=>{changePlan(selSch.id,pl);setSelSch({...selSch,plan:pl,mrr:selSch.students*plPr(pl)});}} style={bo(selSch.plan===pl,true)}>{pl.charAt(0).toUpperCase()+pl.slice(1)}</button>)}</div>
              <button onClick={()=>extendTrial(selSch.id)} style={{...bo(false,true),textAlign:"left",color:"#F59E0B",borderColor:"#F59E0B40",fontSize:11}}>⏰ Extender trial +14d</button>
              <button onClick={()=>exportSchool(selSch.id)} style={{...bo(false,true),textAlign:"left",color:"#0EA5E9",borderColor:"#0EA5E940",fontSize:11}}>⬇ Exportar CSV</button>
              <button onClick={()=>{suspendSchool(selSch.id);setSelSch({...selSch,status:selSch.status==="suspended"?"active":"suspended"});}} style={{...bo(false,true),textAlign:"left",color:selSch.status==="suspended"?"#059669":"#DC2626",fontSize:11}}>{selSch.status==="suspended"?"✅ Reactivar":"⛔ Suspender"}</button>
            </div></div>
          </div>
        </div>}
        {/* ═══ PAYMENTS ═══ */}
        {v==="pay"&&<div>
          <SH title="Pagos y Suscripciones" sub="Facturación, cupones y cobros"/>
          <div style={{display:"grid",gridTemplateColumns:"repeat(4,1fr)",gap:10,marginBottom:14}}>
            <KPI label="MRR" value={"€"+mrr} color="#059669" icon={TrendingUp}/><KPI label="ARR" value={"€"+arr.toLocaleString()} color="#0EA5E9" icon={BarChart2}/><KPI label="Pagadas" value={invoices.filter(i=>i.status==="paid").length} icon={CheckCircle}/><KPI label="Reembolsos" value={invoices.filter(i=>i.status==="refunded").length} color="#DC2626" icon={Activity}/>
          </div>
          <div style={{display:"grid",gridTemplateColumns:"1fr 1fr",gap:12,marginBottom:14}}>
            <div style={{...cd(),padding:14}}>
              <div style={{display:"flex",justifyContent:"space-between",alignItems:"center",marginBottom:8}}><div style={{fontSize:12,fontWeight:700}}>Cupones ({coupons.length})</div><button onClick={()=>setMdl("addCpn")} style={bo(false,true)}>+ Crear</button></div>
              {coupons.map(c=><div key={c.id} style={{display:"flex",justifyContent:"space-between",padding:"6px 0",borderBottom:"1px solid "+BL,alignItems:"center"}}><div><div style={{fontSize:12,fontWeight:700,fontFamily:"monospace",color:PD}}>{c.code}</div><div style={{fontSize:10,color:TM}}>{c.desc} · {c.pct}%</div></div><div style={{display:"flex",alignItems:"center",gap:6}}><span style={{fontSize:10,color:TM}}>{c.uses}/{c.max}</span><button onClick={()=>toggleCoupon(c.id)} style={{fontSize:9,padding:"2px 6px",borderRadius:4,border:"none",cursor:"pointer",background:c.on?"#ECFDF5":"#FEF2F2",color:c.on?"#059669":"#DC2626",fontWeight:700}}>{c.on?"Activo ✕":"Off ↺"}</button></div></div>)}
            </div>
            <div style={{...cd(),padding:14}}><div style={{fontSize:12,fontWeight:700,marginBottom:8}}>Dunning</div>{[{d:"Día 1",a:"Reintento automático"},{d:"Día 3",a:"Reintento + email"},{d:"Día 5",a:"Reintento + notif admin"},{d:"Día 7",a:"Último reintento"},{d:"Día 14",a:"Cancelación auto"}].map(x=><div key={x.d} style={{display:"flex",justifyContent:"space-between",fontSize:11,padding:"4px 0",borderBottom:"1px solid "+BL}}><span style={{fontWeight:600}}>{x.d}</span><span style={{color:TM}}>{x.a}</span></div>)}</div>
          </div>
          <div style={{...cd(),overflow:"hidden"}}>
            <div style={{display:"flex",justifyContent:"space-between",padding:"12px 14px",borderBottom:"1px solid "+BD}}><div style={{fontSize:12,fontWeight:700}}>Facturas ({invoices.length})</div></div>
            <div style={{display:"grid",gridTemplateColumns:"2fr 1fr 1fr 1fr 100px",padding:"8px 14px",background:BL,gap:8}}>{["Colegio","Importe","Fecha","Plan","Acción"].map(h=><div key={h} style={{fontSize:10,fontWeight:700,color:TM,textTransform:"uppercase"}}>{h}</div>)}</div>
            {invoices.map((inv,i)=><div key={inv.id} style={{display:"grid",gridTemplateColumns:"2fr 1fr 1fr 1fr 100px",padding:"10px 14px",gap:8,borderBottom:"1px solid "+BL,background:i%2===0?"#fff":"#FAFBFC",alignItems:"center"}}>
              <div style={{fontSize:12,fontWeight:600}}>{inv.school}</div>
              <div style={{fontSize:12,fontWeight:700,color:inv.status==="refunded"?"#DC2626":inv.amount===0?TL:"#059669"}}>€{inv.amount}{inv.status==="refunded"?" ↩":""}</div>
              <div style={{fontSize:11,color:TM}}>{fmt(inv.dt)}</div>
              <div><Bg text={inv.plan} color={plCo(inv.plan.toLowerCase().split(" ")[0])}/></div>
              <div>{inv.status==="paid"?<button onClick={()=>{if(confirm("¿Reembolsar €"+inv.amount+"?"))refundInv(inv.id);}} style={{fontSize:10,padding:"3px 8px",borderRadius:4,border:"none",cursor:"pointer",background:"#FEF2F2",color:"#DC2626",fontWeight:600}}>Refund</button>:inv.status==="refunded"?<Bg text="Reembolsado" color="#DC2626"/>:<Bg text="Trial" color="#F59E0B"/>}</div>
            </div>)}
          </div>
        </div>}
        {/* ═══ CONTENT ═══ */}
        {v==="content"&&<div>
          <SH title="Contenido Pedagógico" sub="Framework NEXUS™ — 6 dominios · 30 subdimensiones · 96 ítems"/>
          <div style={{...cd(),padding:14,marginBottom:14}}>
            <div style={{display:"flex",justifyContent:"space-between",alignItems:"center",marginBottom:10}}><div style={{fontSize:12,fontWeight:700}}>Estructura NEXUS™</div><div style={{display:"flex",gap:4}}><Bg text={(versions.find(x=>x.status==="published")?.ver||"—")+" Publicada"} color="#059669"/><button onClick={createDraft} style={bo(false,true)}>+ Draft</button></div></div>
            {DOM.map(d=>{const Ic=d.I;return <div key={d.k} style={{marginBottom:8,padding:10,borderRadius:10,border:"1px solid "+d.c+"30",background:d.c+"08"}}><div style={{display:"flex",alignItems:"center",gap:8,marginBottom:6}}><div style={{width:28,height:28,borderRadius:8,background:d.c+"20",display:"flex",alignItems:"center",justifyContent:"center"}}><Ic size={14} color={d.c}/></div><div><div style={{fontSize:13,fontWeight:700}}>{d.l}</div><div style={{fontSize:10,color:TM}}>16 ítems · 5 subdim.</div></div></div><div style={{display:"flex",gap:4,flexWrap:"wrap"}}>{d.sub.map(s=><span key={s} style={{fontSize:10,padding:"3px 8px",background:"#fff",borderRadius:6,border:"1px solid "+BD,color:TM}}>{s}</span>)}</div></div>})}
          </div>
          <div style={{display:"grid",gridTemplateColumns:"1fr 1fr",gap:12}}>
            <div style={{...cd(),padding:14}}>
              <div style={{fontSize:12,fontWeight:700,marginBottom:8}}>Percentiles</div>
              {[3,4,5,6].map(a=><div key={a} style={{display:"flex",justifyContent:"space-between",fontSize:11,padding:"5px 0",borderBottom:"1px solid "+BL}}><span style={{fontWeight:600}}>{a} años</span><span style={{color:TM}}>P10:{PCTL[a].P10} P25:{PCTL[a].P25} P50:{PCTL[a].P50} P90:{PCTL[a].P90}</span></div>)}
              <button onClick={()=>{log("content.pctl_view","Abrió editor percentiles","Contenido");alert("Editor de percentiles: próximamente los cambios se guardarán globalmente. Valores actuales disponibles en Config > Scoring.");}} style={{...bo(false,true),width:"100%",marginTop:8,fontSize:11}}>Editar percentiles</button>
            </div>
            <div style={{...cd(),padding:14}}>
              <div style={{fontSize:12,fontWeight:700,marginBottom:8}}>Versiones</div>
              {versions.map(ver=><div key={ver.id} style={{display:"flex",justifyContent:"space-between",padding:"6px 0",borderBottom:"1px solid "+BL,alignItems:"center"}}><div style={{flex:1}}><div style={{display:"flex",alignItems:"center",gap:6}}><span style={{fontSize:12,fontWeight:700}}>{ver.ver}</span><Bg text={ver.status==="published"?"Publicada":ver.status==="draft"?"Draft":"Archivada"} color={ver.status==="published"?"#059669":ver.status==="draft"?"#F59E0B":"#64748B"}/></div><div style={{fontSize:10,color:TM}}>{ver.author} · {fmt(ver.dt)}{ver.notes?" · "+ver.notes:""}</div></div><div style={{display:"flex",gap:3}}>{ver.status==="draft"&&<><input value={ver.notes||""} onChange={e=>updateVerNotes(ver.id,e.target.value)} placeholder="Notas..." style={{...inp,width:100,padding:"3px 6px",fontSize:10}} onClick={e=>e.stopPropagation()}/><button onClick={()=>publishVer(ver.id)} style={{fontSize:9,padding:"3px 8px",borderRadius:4,border:"none",cursor:"pointer",background:"#ECFDF5",color:"#059669",fontWeight:700}}>Publicar</button></>}{ver.status==="archived"&&<button onClick={()=>rollbackVer(ver.id)} style={{fontSize:9,padding:"3px 8px",borderRadius:4,border:"none",cursor:"pointer",background:"#FFFBEB",color:"#D97706",fontWeight:700}}>Rollback</button>}</div></div>)}
            </div>
          </div>
        </div>}
        {/* ═══ AUDIT ═══ */}
        {v==="audit"&&<div>
          <SH title={"Auditoría ("+auditLogs.length+")"} sub="Registro inmutable" right={<><button onClick={exportAudit} style={bo(false,true)}>⬇ CSV</button><button onClick={()=>setAuditF("all")} style={bo(false,true)}>Limpiar</button></>}/>
          <div style={{display:"flex",gap:4,marginBottom:12,flexWrap:"wrap"}}>{[{k:"all",l:"Todo"},{k:"user",l:"Usuarios"},{k:"eval",l:"Evals"},{k:"payment",l:"Pagos"},{k:"alert",l:"Alertas"},{k:"school",l:"Colegios"},{k:"content",l:"Contenido"},{k:"system",l:"Sistema"},{k:"config",l:"Config"}].map(f=><button key={f.k} onClick={()=>setAuditF(f.k)} style={bo(auditF===f.k,true)}>{f.l}</button>)}</div>
          <div style={{...cd(),overflow:"hidden"}}>
            <div style={{display:"grid",gridTemplateColumns:"150px 120px 1fr 100px",padding:"8px 14px",background:DARK,gap:8}}>{["Fecha","Actor","Detalle","IP"].map(h=><div key={h} style={{fontSize:10,fontWeight:700,color:"#fff",textTransform:"uppercase"}}>{h}</div>)}</div>
            {filtAudit.length===0?<div style={{padding:30,textAlign:"center",color:TL}}>Sin registros</div>:filtAudit.slice(0,50).map((a,i)=><div key={a.id+""+i} style={{display:"grid",gridTemplateColumns:"150px 120px 1fr 100px",padding:"10px 14px",gap:8,borderBottom:"1px solid "+BL,background:i%2===0?"#fff":"#FAFBFC"}}>
              <div style={{fontSize:11,color:TM}}>{a.dt.replace("T"," ").slice(0,16)}</div>
              <div style={{fontSize:11,fontWeight:600}}>{a.actor}</div>
              <div style={{display:"flex",alignItems:"center",gap:6}}><div style={{width:6,height:6,borderRadius:3,background:a.action.includes("delete")||a.action.includes("suspend")?"#DC2626":a.action.includes("payment")||a.action.includes("publish")?"#059669":"#0EA5E9",flexShrink:0}}/><div><div style={{fontSize:11}}>{a.detail}</div><div style={{fontSize:9,color:TL,fontFamily:"monospace"}}>{a.action}</div></div></div>
              <div style={{fontSize:10,color:TL,fontFamily:"monospace"}}>{a.ip}</div>
            </div>)}
          </div>
        </div>}
        {/* ═══ FLAGS ═══ */}
        {v==="flags"&&<div>
          <SH title="Feature Flags" sub="Sin despliegue"/>
          <div style={{...cd(),overflow:"hidden"}}>{[{k:"ai_predictive",l:"IA Predictiva",desc:"Predicción TDAH, ansiedad, dif. aprendizaje",risk:"alto"},{k:"parent_portal",l:"Portal Familias",desc:"Acceso padres: perfil hijo, actividades, mensajería",risk:"bajo"},{k:"screening_v2",l:"Screening v2",desc:"Matriz screening con percentiles por edad",risk:"medio"},{k:"guided_obs",l:"Observación Guiada",desc:"Actividades observación con puntuación e impacto",risk:"bajo"},{k:"pdf_quincenal",l:"PDF Quincenal",desc:"Informes PDF quincenales automáticos",risk:"bajo"},{k:"maintenance",l:"Mantenimiento",desc:"Página mantenimiento (excepto admins)",risk:"alto"}].map((f,i)=><div key={f.k} style={{display:"flex",justifyContent:"space-between",alignItems:"center",padding:"14px 16px",borderBottom:"1px solid "+BL,background:i%2===0?"#fff":"#FAFBFC"}}><div style={{flex:1}}><div style={{display:"flex",alignItems:"center",gap:8}}><span style={{fontSize:13,fontWeight:700}}>{f.l}</span><span style={{fontSize:9,padding:"2px 6px",borderRadius:4,background:f.risk==="alto"?"#FEF2F2":f.risk==="medio"?"#FFFBEB":"#ECFDF5",color:f.risk==="alto"?"#DC2626":f.risk==="medio"?"#D97706":"#059669",fontWeight:600}}>Riesgo {f.risk}</span></div><div style={{fontSize:11,color:TM,marginTop:2}}>{f.desc}</div></div><Tog on={flags[f.k]} onClick={()=>toggleFlag(f.k)}/></div>)}</div>
          <div style={{...cd(),padding:14,marginTop:14}}><div style={{fontSize:12,fontWeight:700,marginBottom:8}}>Parámetros</div><div style={{display:"grid",gridTemplateColumns:"1fr 1fr",gap:10}}>{[{k:"max_trial_days",l:"Días trial"},{k:"eval_frequency",l:"Frecuencia eval (días)"}].map(p2=><div key={p2.k}><label style={{fontSize:11,fontWeight:600,color:TM,display:"block",marginBottom:4}}>{p2.l}</label><input type="number" value={flags[p2.k]||0} onChange={e=>setFlags({...flags,[p2.k]:parseInt(e.target.value)||0})} style={{...inp,padding:"8px 12px"}}/></div>)}</div></div>
        </div>}
        {/* ═══ CONFIG ═══ */}
        {v==="config"&&<div>
          <SH title="Configuración" sub="Sistema, legal, emails, scoring, idiomas"/>
          <div style={{display:"flex",gap:4,marginBottom:14}}>{[{k:"general",l:"General"},{k:"legal",l:"Legal"},{k:"emails",l:"Emails"},{k:"scoring",l:"Scoring"},{k:"i18n",l:"Idiomas"}].map(t=><button key={t.k} onClick={()=>setConfTab(t.k)} style={bo(confTab===t.k,true)}>{t.l}</button>)}</div>
          {confTab==="general"&&<div style={{...cd(),padding:16}}><div style={{fontSize:12,fontWeight:700,marginBottom:12}}>Datos del Centro</div><div style={{display:"grid",gridTemplateColumns:"1fr 1fr",gap:12}}><div><label style={{fontSize:11,fontWeight:600,color:TM,display:"block",marginBottom:4}}>Centro</label><input value={cfg.schoolName} onChange={e=>{setCfg("schoolName",e.target.value);up({school:{...D.school,name:e.target.value}});}} style={inp}/></div><div><label style={{fontSize:11,fontWeight:600,color:TM,display:"block",marginBottom:4}}>País</label><select value={cfg.country} onChange={e=>setCfg("country",e.target.value)} style={inp}><option value="ES">España</option><option value="PT">Portugal</option><option value="FR">Francia</option><option value="IT">Italia</option></select></div><div><label style={{fontSize:11,fontWeight:600,color:TM,display:"block",marginBottom:4}}>Email</label><input value={cfg.email} onChange={e=>setCfg("email",e.target.value)} style={inp}/></div><div><label style={{fontSize:11,fontWeight:600,color:TM,display:"block",marginBottom:4}}>Zona horaria</label><select value={cfg.tz} onChange={e=>setCfg("tz",e.target.value)} style={inp}><option value="Europe/Madrid">Madrid (CET)</option><option value="Europe/Lisbon">Lisboa (WET)</option><option value="Europe/Paris">París (CET)</option></select></div></div><button onClick={()=>{log("config.save","Guardó config general","Config");alert("✓ Guardado");}} style={{...bp(true),marginTop:14}}>Guardar</button></div>}
          {confTab==="legal"&&<div style={{...cd(),padding:16}}><div style={{fontSize:12,fontWeight:700,marginBottom:12}}>Textos Legales</div>{[{l:"Términos de Servicio",k:"tos"},{l:"Política de Privacidad",k:"priv"},{l:"Consentimiento RGPD",k:"rgpd"},{l:"DPA",k:"dpa"}].map(t=><div key={t.k} style={{padding:"10px 0",borderBottom:"1px solid "+BL}}><div style={{display:"flex",justifyContent:"space-between",alignItems:"center",marginBottom:6}}><span style={{fontSize:12,fontWeight:600}}>{t.l}</span><Bg text={"Act: "+td()} color="#059669"/></div><textarea defaultValue={"["+t.l+" — editar aquí]"} rows={2} style={{...inp,fontSize:11,resize:"vertical"}} onBlur={()=>log("config.legal","Editó: "+t.l,"Config")}/></div>)}</div>}
          {confTab==="emails"&&<div style={{...cd(),padding:16}}><div style={{fontSize:12,fontWeight:700,marginBottom:12}}>Plantillas Email</div>{emails.map(e2=><div key={e2.id} style={{display:"flex",justifyContent:"space-between",alignItems:"center",padding:"10px 0",borderBottom:"1px solid "+BL}}><div><div style={{fontSize:12,fontWeight:600}}>{e2.l}</div><div style={{fontSize:10,color:TM}}>{e2.desc}</div></div><Tog on={e2.on} onClick={()=>toggleEmail(e2.id)} sz={42}/></div>)}</div>}
          {confTab==="scoring"&&<div style={{...cd(),padding:16}}><div style={{fontSize:12,fontWeight:700,marginBottom:12}}>Scoring</div><div style={{display:"grid",gridTemplateColumns:"1fr 1fr",gap:10}}><div><label style={{fontSize:11,fontWeight:600,color:"#DC2626",display:"block",marginBottom:4}}>Umbral P10 (Derivación)</label><input type="number" value={cfg.p10} onChange={e=>setCfg("p10",parseInt(e.target.value)||0)} style={inp}/></div><div><label style={{fontSize:11,fontWeight:600,color:"#D97706",display:"block",marginBottom:4}}>Umbral P25 (Seguimiento)</label><input type="number" value={cfg.p25} onChange={e=>setCfg("p25",parseInt(e.target.value)||0)} style={inp}/></div><div><label style={{fontSize:11,fontWeight:600,color:"#059669",display:"block",marginBottom:4}}>Umbral P90 (Excelente)</label><input type="number" value={cfg.p90} onChange={e=>setCfg("p90",parseInt(e.target.value)||0)} style={inp}/></div><div><label style={{fontSize:11,fontWeight:600,display:"block",marginBottom:4}}>Escala</label><select value={cfg.scale} onChange={e=>setCfg("scale",e.target.value)} style={inp}><option value="likert04">Likert 0-4</option><option value="likert05">Likert 0-5</option><option value="numeric">0-100</option></select></div></div><button onClick={()=>{log("config.scoring","Scoring: P10="+cfg.p10+" P25="+cfg.p25+" P90="+cfg.p90,"Config");alert("✓ Scoring guardado");}} style={{...bp(true),marginTop:14}}>Guardar</button></div>}
          {confTab==="i18n"&&<div style={{...cd(),padding:16}}><div style={{fontSize:12,fontWeight:700,marginBottom:12}}>Idiomas</div>{langs.map(l=><div key={l.code} style={{display:"flex",justifyContent:"space-between",alignItems:"center",padding:"10px 0",borderBottom:"1px solid "+BL}}><div style={{display:"flex",alignItems:"center",gap:10}}><div style={{width:28,height:20,borderRadius:3,background:l.on?P+"15":"#F1F5F9",display:"flex",alignItems:"center",justifyContent:"center",fontSize:10,fontWeight:700,color:l.on?PD:TL}}>{l.code}</div><div><div style={{fontSize:12,fontWeight:600}}>{l.name}</div><div style={{fontSize:10,color:TM}}>{l.on?l.keys+" claves":"Pendiente"}</div></div></div><Tog on={l.on} onClick={()=>toggleLang(l.code)} sz={42}/></div>)}</div>}
        </div>}
      </div>
      {mdl==="addU"&&<Mdl title="Nuevo usuario" onClose={()=>setMdl(null)}><AdminAddUser D={D} up={up} onClose={()=>setMdl(null)} log={log}/></Mdl>}
      {mdl==="editU"&&editU&&<Mdl title={"Editar: "+editU.nm} onClose={()=>{setMdl(null);setEditU(null);}}><AdminEditUser u={editU} D={D} up={up} onClose={()=>{setMdl(null);setEditU(null);}} log={log}/></Mdl>}
      {mdl==="addSch"&&<Mdl title="Nuevo colegio" onClose={()=>setMdl(null)}><AdminAddSchool onSave={d=>{addSchool(d);setMdl(null);}} onClose={()=>setMdl(null)}/></Mdl>}
      {mdl==="addCpn"&&<Mdl title="Nuevo cupón" onClose={()=>setMdl(null)}><AdminAddCoupon onSave={d=>{addCoupon(d);setMdl(null);}} onClose={()=>setMdl(null)}/></Mdl>}
    </div>
  );
}
function AdminAddUser({D,up,onClose,log}){
  const [n,setN]=useState("");const [e,setE]=useState("");const [r,setR]=useState("teacher");const [p,setP]=useState("1234");
  const save=()=>{if(!n.trim()||!e.trim())return;if(r==="parent"){up({parents:[...(D.parents||[]),{id:uid(),nm:n.trim(),em:e.trim(),pin:p,ch:[]}]});}else{up({users:[...(D.users||[]),{id:uid(),nm:n.trim(),em:e.trim(),role:r,pin:p}]});}log("user.create","Creó "+r+": "+n.trim(),"Usuario");onClose();};
  return <div><div style={{display:"flex",gap:5,marginBottom:8}}>{["teacher","psico","parent","admin"].map(x=><button key={x} onClick={()=>setR(x)} style={bo(r===x,true)}>{x==="teacher"?"Docente":x==="psico"?"Psico":x==="parent"?"Familia":"Admin"}</button>)}</div><input value={n} onChange={ev=>setN(ev.target.value)} placeholder="Nombre completo" style={{...inp,marginBottom:6}}/><input value={e} onChange={ev=>setE(ev.target.value)} placeholder="Email" style={{...inp,marginBottom:6}}/><input value={p} onChange={ev=>setP(ev.target.value)} placeholder="PIN" style={{...inp,marginBottom:10}}/><button onClick={save} disabled={!n.trim()||!e.trim()} style={{...bp(),width:"100%",opacity:n.trim()&&e.trim()?1:.5}}>Crear</button></div>;
}
function AdminEditUser({u,D,up,onClose,log}){
  const [n,setN]=useState(u.nm);const [e,setE]=useState(u.em);const [r,setR]=useState(u.role||"parent");
  const save=()=>{if(!n.trim()||!e.trim())return;const ip=!u.role||u.role==="parent";if(ip){up({parents:(D.parents||[]).map(p2=>p2.id===u.id?{...p2,nm:n.trim(),em:e.trim()}:p2)});}else{up({users:(D.users||[]).map(u2=>u2.id===u.id?{...u2,nm:n.trim(),em:e.trim(),role:r}:u2)});}log("user.update","Editó: "+u.nm+" → "+n.trim(),"Usuario");onClose();};
  return <div><div style={{display:"flex",gap:5,marginBottom:8}}>{["teacher","psico","admin"].map(x=><button key={x} onClick={()=>setR(x)} style={bo(r===x,true)}>{x==="teacher"?"Docente":x==="psico"?"Psico":"Admin"}</button>)}</div><input value={n} onChange={ev=>setN(ev.target.value)} placeholder="Nombre" style={{...inp,marginBottom:6}}/><input value={e} onChange={ev=>setE(ev.target.value)} placeholder="Email" style={{...inp,marginBottom:10}}/><button onClick={save} disabled={!n.trim()||!e.trim()} style={{...bp(),width:"100%",opacity:n.trim()&&e.trim()?1:.5}}>Guardar</button></div>;
}
function AdminAddSchool({onSave,onClose}){
  const [nm,setNm]=useState("");const [co,setCo]=useState("ES");const [pl,setPl]=useState("basic");const [st,setSt]=useState("");const [te,setTe]=useState("");const [ct,setCt]=useState("");
  return <div><input value={nm} onChange={e=>setNm(e.target.value)} placeholder="Nombre del colegio" style={{...inp,marginBottom:6}}/><div style={{display:"grid",gridTemplateColumns:"1fr 1fr",gap:6,marginBottom:6}}><select value={co} onChange={e=>setCo(e.target.value)} style={inp}><option value="ES">España</option><option value="PT">Portugal</option><option value="FR">Francia</option><option value="IT">Italia</option></select><select value={pl} onChange={e=>setPl(e.target.value)} style={inp}><option value="basic">Basic</option><option value="standard">Standard</option><option value="premium">Premium</option></select></div><div style={{display:"grid",gridTemplateColumns:"1fr 1fr",gap:6,marginBottom:6}}><input value={st} onChange={e=>setSt(e.target.value)} placeholder="Nº alumnos" type="number" style={inp}/><input value={te} onChange={e=>setTe(e.target.value)} placeholder="Nº docentes" type="number" style={inp}/></div><input value={ct} onChange={e=>setCt(e.target.value)} placeholder="Email contacto" style={{...inp,marginBottom:10}}/><button onClick={()=>{if(!nm.trim())return;onSave({nm:nm.trim(),country:co,plan:pl,students:st,teachers:te,contact:ct});}} disabled={!nm.trim()} style={{...bp(),width:"100%",opacity:nm.trim()?1:.5}}>Crear colegio</button></div>;
}
function AdminAddCoupon({onSave,onClose}){
  const [code,setCode]=useState("");const [desc,setDesc]=useState("");const [pct,setPct]=useState("20");const [mx,setMx]=useState("10");
  return <div><input value={code} onChange={e=>setCode(e.target.value.toUpperCase())} placeholder="CÓDIGO" style={{...inp,marginBottom:6,fontFamily:"monospace",letterSpacing:2}}/><input value={desc} onChange={e=>setDesc(e.target.value)} placeholder="Descripción" style={{...inp,marginBottom:6}}/><div style={{display:"grid",gridTemplateColumns:"1fr 1fr",gap:6,marginBottom:10}}><input value={pct} onChange={e=>setPct(e.target.value)} placeholder="% descuento" type="number" style={inp}/><input value={mx} onChange={e=>setMx(e.target.value)} placeholder="Usos máx" type="number" style={inp}/></div><button onClick={()=>{if(!code.trim())return;onSave({code,desc,pct,max:mx});}} disabled={!code.trim()} style={{...bp(),width:"100%",opacity:code.trim()?1:.5}}>Crear cupón</button></div>;
}
/* ═══ PSICO ═══ */
function PsicoApp({D,up,logout}){
  const auth=D.auth||{};const user=(D.users||[]).find(u=>u.id===auth.uid)||{nm:"Psico"};
  const [v,setV]=useState("dash");
  const ref=(D.sts||[]).filter(s=>(D.als||[]).some(a=>a.sid===s.id&&a.st==="active"&&a.sev==="high"));
  const navs=[{k:"dash",l:"Dashboard",I:BarChart3},{k:"ref",l:"Derivados",I:AlertTriangle,badge:ref.length},{k:"all",l:"Alumnos",I:Users}];
  return (
    <div style={{display:"flex",minHeight:"100vh",background:BG,fontFamily:F}}>
      <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;500;600;700;800&family=DM+Serif+Display&display=swap" rel="stylesheet"/>
      <SNav items={navs} active={v} onNav={setV} sub="" userName={user.nm||""} onLogout={logout}/>
      <div style={{flex:1,padding:18,overflowY:"auto",maxWidth:880,margin:"0 auto"}}>
        {v==="dash" && <div><h2 style={{fontFamily:FD,fontSize:20,margin:"0 0 12px"}}>Panel Psicopedagógico</h2><div style={{display:"grid",gridTemplateColumns:"1fr 1fr",gap:10}}>{[{l:"Derivados",v:ref.length},{l:"Total alumnos",v:(D.sts||[]).length}].map((k,i)=><div key={i} style={{...cd(),padding:12}}><div style={{fontFamily:FD,fontSize:22}}>{k.v}</div><div style={{fontSize:11,color:TM}}>{k.l}</div></div>)}</div></div>}
        {v==="ref" && <div><h2 style={{fontFamily:FD,fontSize:20,margin:"0 0 12px"}}>Derivados</h2>{ref.length===0?<div style={{...cd(),padding:30,textAlign:"center"}}><p style={{color:TL}}>Sin derivados</p></div>:ref.map(s=>{const ev=(D.evs||[]).filter(e=>e.sid===s.id).sort((a,b)=>b.dt.localeCompare(a.dt))[0];return <div key={s.id} style={{...cd(),padding:12,marginBottom:6}}><div style={{display:"flex",alignItems:"center",gap:8}}><Av nm={s.nm} sz={30}/><div style={{flex:1}}><div style={{fontSize:13,fontWeight:600}}>{s.nm}</div><div style={{fontSize:11,color:TL}}>{ageFn(s.dob)}</div></div>{ev&&<span style={{fontWeight:700,color:percCo(gs(ev.sc))}}>{gs(ev.sc)}%</span>}</div>{ev&&<Bars sc={ev.sc} sm={true}/>}</div>})}</div>}
        {v==="all" && <div><h2 style={{fontFamily:FD,fontSize:20,margin:"0 0 12px"}}>Todos</h2>{(D.sts||[]).map(s=><div key={s.id} style={{...cd(),padding:10,marginBottom:5}}><div style={{display:"flex",alignItems:"center",gap:8}}><Av nm={s.nm} sz={28}/><div style={{fontSize:12,fontWeight:600}}>{s.nm}</div></div></div>)}</div>}
      </div>
    </div>
  );
}

/* ═══ MAIN ═══ */
export default function App(){
  const [D,setD]=useState(()=>({auth:null,school:{name:""},cls:[],sts:[],evs:[],obs:[],als:[],pmi:[],users:[],parents:[],msgs:[],journal:[],pNotes:[],psNotes:[],hDiary:[],milestones:[],dailyAn:[],actAssign:[],incidents:[],invitations:[],guidedEvals:[],customActs:[],favActs:[],lic:{plan:"piloto",maxSt:50}}));
  const [mode,setMode]=useState("login");
  const [err,setErr]=useState("");
  const [em,setEm]=useState("");
  const [pin,setPin]=useState("");
  const [role,setRole]=useState("teacher");
  const [rNm,setRNm]=useState("");
  const [rEm,setREm]=useState("");
  const [rPin,setRPin]=useState("");
  const [rRole,setRRole]=useState("teacher");
  const [rSchool,setRSchool]=useState("");
  const [rCode,setRCode]=useState("");

  const up=p=>setD(prev=>({...prev,...p}));
  const logout=()=>{up({auth:null});setErr("");};

  const doLogin=()=>{
    setErr("");
    if(!em.trim()||!pin.trim()){setErr("Rellena email y PIN");return;}
    if(em.trim().toLowerCase()==="admin"&&pin==="admin"){up({auth:{role:"admin",uid:"superadmin"}});return;}
    const par=(D.parents||[]).find(x=>x.em===em);
    if(par){if(par.pin!==pin){setErr("PIN incorrecto");return;}up({auth:{role:"parent",parId:par.id}});return;}
    const u=(D.users||[]).find(x=>x.em===em);
    if(!u){setErr("Email no encontrado");return;}
    if(u.pin!==pin){setErr("PIN incorrecto");return;}
    up({auth:{role:u.role,uid:u.id}});
  };

  const doRegister=()=>{
    setErr("");
    if(!rNm.trim()||!rEm.trim()||!rPin.trim()){setErr("Rellena todos");return;}
    if((D.users||[]).find(u=>u.em===rEm)||(D.parents||[]).find(p=>p.em===rEm)){setErr("Email ya existe");return;}
    if(rPin.length<4){setErr("PIN mín 4 chars");return;}
    const id=uid();
    if(rRole==="parent"){const id2=uid();const ch=[];const newInvs=[...(D.invitations||[])];if(rCode.trim()){const inv=newInvs.find(i=>i.code===rCode.trim().toUpperCase()&&i.status==="pending");if(inv){inv.parId=id2;inv.status="accepted";ch.push(inv.sid);}else{setErr("Código de invitación no válido");return;}}up({parents:[...(D.parents||[]),{id:id2,nm:rNm.trim(),em:rEm.trim(),pin:rPin,ch}],invitations:newInvs,auth:{role:"parent",parId:id2}});}
    else{const ch={users:[...(D.users||[]),{id,nm:rNm.trim(),em:rEm.trim(),role:rRole,pin:rPin}],auth:{role:rRole,uid:id}};if(rRole==="admin"&&rSchool.trim())ch.school={name:rSchool.trim()};up(ch);}
  };

  const rl=D.auth?.role||null;
  const ROLES=[{k:"teacher",l:"Docente",I:GraduationCap},{k:"parent",l:"Familia",I:Home},{k:"psico",l:"Psico",I:Shield}];

  return (
    <div style={{fontFamily:F}}>
      <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;500;600;700;800&family=DM+Serif+Display&display=swap" rel="stylesheet"/>
      {rl==="teacher" && <TeacherApp D={D} up={up} logout={logout}/>}
      {rl==="parent" && <ParentApp D={D} up={up} logout={logout}/>}
      {rl==="psico" && <PsicoApp D={D} up={up} logout={logout}/>}
      {rl==="admin" && <AdminApp D={D} up={up} logout={logout}/>}
      {!rl && <div style={{minHeight:"100vh",display:"flex",alignItems:"center",justifyContent:"center",background:"linear-gradient(135deg,"+BG+",#F0FDFA)",padding:16}}>
        <div style={{width:"100%",maxWidth:400}}>
          <div style={{textAlign:"center",marginBottom:20}}><div style={{display:"flex",justifyContent:"center",marginBottom:8}}><NLogo sz={48}/></div><h1 style={{fontFamily:FD,fontSize:24,color:TX,margin:0}}>NEXUS KIDS</h1><p style={{color:TM,fontSize:13,margin:"3px 0 0"}}>{"Plataforma de desarrollo infantil"}</p></div>
                    <div style={cd({padding:22})}>
            <div style={{display:"flex",marginBottom:14,borderBottom:"2px solid "+BL}}>
              <button onClick={()=>{setMode("login");setErr("");}} style={{flex:1,padding:"9px 0",background:"none",border:"none",borderBottom:mode==="login"?"2px solid "+P:"2px solid transparent",color:mode==="login"?PD:TM,fontFamily:F,fontSize:13,fontWeight:mode==="login"?700:400,cursor:"pointer",marginBottom:-2}}>{"Acceder"}</button>
              <button onClick={()=>{setMode("register");setErr("");}} style={{flex:1,padding:"9px 0",background:"none",border:"none",borderBottom:mode==="register"?"2px solid "+P:"2px solid transparent",color:mode==="register"?PD:TM,fontFamily:F,fontSize:13,fontWeight:mode==="register"?700:400,cursor:"pointer",marginBottom:-2}}>{"Registrarse"}</button>
            </div>
            {mode==="login"?<div>
              <input value={em} onChange={e=>setEm(e.target.value)} placeholder="Email" style={{...inp,marginBottom:8}}/>
              <input value={pin} onChange={e=>setPin(e.target.value)} placeholder="PIN" type="password" style={{...inp,marginBottom:10}}/>
              {err&&<div style={{fontSize:12,color:"#DC2626",marginBottom:8,textAlign:"center"}}>{err}</div>}
              <button onClick={doLogin} style={{...bp(),width:"100%"}}>{"Acceder"}</button>
              <div style={{textAlign:"center",marginTop:10}}><button onClick={()=>{const d=mkDemo();up(d);setEm("maria@colegio.es");setPin("1234");}} style={{background:"none",border:"none",color:P,fontSize:11,cursor:"pointer",fontFamily:F}}>{"Cargar datos de demo"}</button></div>
            </div>:<div>
              <div style={{display:"flex",gap:4,marginBottom:10}}>{ROLES.map(r=>{const Ic=r.I;return <button key={r.k} onClick={()=>setRRole(r.k)} style={{...bo(rRole===r.k,true),flex:1,display:"flex",flexDirection:"column",alignItems:"center",gap:2,padding:"8px 4px"}}><Ic size={14}/><span style={{fontSize:10}}>{r.l}</span></button>})}</div>
              <input value={rNm} onChange={e=>setRNm(e.target.value)} placeholder={"Nombre completo"} style={{...inp,marginBottom:6}}/>
              <input value={rEm} onChange={e=>setREm(e.target.value)} placeholder="Email" style={{...inp,marginBottom:6}}/>
              <input value={rPin} onChange={e=>setRPin(e.target.value)} placeholder={""} type="password" style={{...inp,marginBottom:6}}/>
              {rRole==="admin"&&<input value={rSchool} onChange={e=>setRSchool(e.target.value)} placeholder={"Nombre del centro"} style={{...inp,marginBottom:6}}/>}
              {rRole==="parent"&&<div><input value={rCode} onChange={e=>setRCode(e.target.value.toUpperCase())} placeholder={""} style={{...inp,marginBottom:6,fontFamily:"monospace",letterSpacing:2,textAlign:"center",background:"#FFFBEB",border:"1.5px solid #F59E0B"}}/>
              {(()=>{const inv=(D.invitations||[]).find(i=>i.code===rCode.trim().toUpperCase()&&i.status==="pending"&&!i.parId);if(!rCode.trim()||!inv)return <div style={{fontSize:10,color:TM,marginBottom:6,textAlign:"center"}}>{"Introduce un código de invitación"}</div>;const st=(D.sts||[]).find(s=>s.id===inv.sid);const tea=(D.users||[]).find(u=>u.id===inv.tid);return <div style={{padding:10,background:PL,borderRadius:10,marginBottom:8,border:"1.5px solid "+P}}><div style={{display:"flex",alignItems:"center",gap:8}}><div style={{width:36,height:36,borderRadius:99,background:P+"20",display:"flex",alignItems:"center",justifyContent:"center"}}><UserPlus size={16} color={P}/></div><div><div style={{fontSize:12,fontWeight:700,color:PD}}>{"Invitación verificada ✓"}</div><div style={{fontSize:11,color:TX,marginTop:1}}>{"Alumno/a: "}<b>{st?.nm||"—"}</b></div>{tea&&<div style={{fontSize:10,color:TM}}>{"Docente"}: {tea.nm}</div>}</div></div></div>})()}</div>}
              {err&&<div style={{fontSize:12,color:"#DC2626",marginBottom:8,textAlign:"center"}}>{err}</div>}
              <button onClick={doRegister} style={{...bp(),width:"100%"}}>{"Crear cuenta"}</button>
            </div>}
          </div>
        </div>
      </div>}
    </div>
  );
}
