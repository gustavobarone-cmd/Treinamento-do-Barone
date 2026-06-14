# Codex Prompt — Personal Trainer Barone

Use este arquivo como prompt mestre no Codex para evoluir o app **Treinamento do Barone**.

## Comandos para sincronizar no PC

Na pasta local do projeto, rode:

```bash
git status
git remote -v
git branch --show-current
git pull origin main
```

Se aparecerem alterações locais que você não quer perder:

```bash
git status
git add .
git commit -m "chore: save local work before sync"
git pull origin main
```

Se você só quer guardar temporariamente:

```bash
git stash push -m "backup antes de sincronizar"
git pull origin main
git stash pop
```

Depois, abra o projeto no Codex e use o prompt abaixo.

---

# Prompt mestre para Codex

Você está trabalhando no repositório `gustavobarone-cmd/Treinamento-do-Barone`.

## Objetivo do produto

Transformar este app em um **personal trainer digital personalizado para Gustavo Barone**, inspirado na experiência de apps como Runtastic/adidas Training, mas sem copiar marca, layout, conteúdo, vídeos ou textos proprietários.

O app deve ser original, mobile-first, prático e focado em:

- treino guiado;
- histórico de treino;
- progressão de carga;
- adaptação por equipamento disponível;
- segurança para ombro;
- registro de carga, reps, RPE, dor e técnica;
- recomendação de próximo treino.

## Contexto do atleta

Nome: Gustavo Barone.

Preferências:

- Estilo preferido: full body.
- Duração ideal: 60 minutos.
- Estrutura ideal: 10 min aquecimento, 40 min treino principal, 5 min desacelerar, 5 min alongamento.
- Objetivos: força, condicionamento, saúde e consistência.

Restrição importante:

- Ombro sensível / histórico de lesão de ombro.
- Evitar movimentos explosivos ou pesados acima da cabeça.
- Bloquear ou alertar fortemente: push press, thruster, snatch, devil press, swing americano, overhead lunge, walking wall e burpees agressivos quando houver dor/desconforto.

Equipamentos disponíveis:

- Rack: sim.
- Barra longa: sim.
- Anilhas: sim.
- Barra W: sim.
- Barra reta: sim.
- Barra fixa: sim.
- Step: sim.
- Banco convencional: não.
- Kettlebells: 22 kg e 26 kg.
- Halter montado: aproximadamente 5 kg de cada lado.

Capacidades conhecidas:

- Back squat: 10 reps fáceis com 30 kg de cada lado.
- Barra fixa: 10 reps em pegada prona ou supina.
- Flexão: até 30 reps.
- Rosca direta: sabe fazer com barra reta ou barra W.
- Stiff / levantamento terra romeno: sabe fazer ou está familiarizado.
- Supino: precisa ser adaptado por ausência de banco; usar floor press, flexão, flexão inclinada no step/rack ou supino adaptado no step, se seguro.

## Stack atual esperada

- Frontend: React + Vite.
- Backend: Node.js + Express.
- Banco: SQLite via better-sqlite3.
- Já existe estrutura de treinos em JSON.
- Já existe tela de execução de treino, logs de série, cronômetro, descanso, vídeo, beep/voz e registros básicos de carga/reps/observação.

## Regras antes de programar

1. Inspecione a estrutura atual do projeto antes de alterar.
2. Não reescreva o projeto inteiro.
3. Preserve funcionalidades existentes.
4. Preserve os treinos do Alberto e o formato atual sempre que possível.
5. Faça mudanças pequenas, coesas e fáceis de revisar.
6. Atualize README ou documentação quando necessário.
7. Não implemente todas as fases de uma vez.
8. Priorize dados estruturados antes de IA.
9. Não copiar conteúdo proprietário de Runtastic/adidas.

## Meta funcional

O app deve permitir que Gustavo:

1. Abra o celular.
2. Responda como está hoje: tempo, energia, dor no ombro, local e objetivo.
3. Receba ou execute um treino adequado.
4. Siga o treino guiado por blocos.
5. Registre carga, reps, RPE, dor e técnica.
6. Receba sugestão objetiva para o próximo treino.

---

# Fase 1 — Perfil do atleta e equipamentos

Criar suporte no backend e frontend para ficha do atleta.

## Dados mínimos do perfil

- nome;
- objetivo principal;
- duração preferida do treino;
- estilo preferido de treino;
- frequência semanal desejada;
- restrições/lesões;
- observações;
- preferências de divisão do treino.

Seed inicial:

```json
{
  "athlete_id": "barone",
  "name": "Gustavo Barone",
  "goals": ["forca", "condicionamento", "saude", "consistencia"],
  "preferred_training_style": "full_body",
  "preferred_duration_min": 60,
  "preferred_structure": {
    "warmup_min": 10,
    "main_workout_min": 40,
    "cooldown_min": 5,
    "stretching_min": 5
  },
  "injuries": [
    {
      "region": "ombro",
      "status": "sensivel",
      "avoid": [
        "push_press",
        "thruster",
        "snatch",
        "devil_press",
        "american_swing",
        "overhead_lunge",
        "walking_wall",
        "overhead_explosive"
      ]
    }
  ]
}
```

## Equipamentos iniciais

```json
{
  "athlete_id": "barone",
  "location": "casa",
  "equipment": {
    "rack": true,
    "barbell": true,
    "plates": true,
    "ez_bar": true,
    "straight_bar": true,
    "pullup_bar": true,
    "step": true,
    "bench": false,
    "kettlebells_kg": [22, 26],
    "dumbbells": [
      {
        "description": "halter montado",
        "load_each_side_kg": 5
      }
    ]
  }
}
```

Criar/adaptar telas:

- Meu Perfil.
- Meus Equipamentos.

Critérios de aceite:

- Perfil pode ser visualizado/editado.
- Equipamentos podem ser visualizados/editados.
- Dados persistem no backend/SQLite ou na estratégia atual do backend.
- Existe seed inicial para Gustavo.
- README explica como rodar e como usar.

---

# Fase 2 — Biblioteca de exercícios inteligente

Criar biblioteca de exercícios com tags úteis para o motor de treino.

Campos mínimos:

- id;
- nome;
- padrão de movimento;
- grupos musculares;
- equipamentos necessários;
- nível técnico;
- risco para ombro: baixo, médio, alto;
- overhead: true/false;
- explosivo: true/false;
- substitutos;
- instrução curta;
- vídeo opcional.

Exercícios iniciais:

- back_squat;
- romanian_deadlift;
- stiff;
- sumo_deadlift;
- pullup_pronated;
- pullup_supinated;
- pushup;
- incline_pushup;
- floor_press;
- barbell_row;
- kettlebell_row;
- goblet_squat;
- bulgarian_split_squat;
- barbell_curl;
- ez_bar_curl;
- plank;
- side_plank;
- glute_bridge;
- dead_bug;
- chest_stretch;
- hamstring_stretch;
- lat_stretch.

Exemplo:

```json
{
  "id": "back_squat",
  "name": "Back Squat",
  "movement_pattern": "agachar",
  "muscle_groups": ["quadriceps", "gluteos", "core"],
  "required_equipment": ["rack", "barbell", "plates"],
  "skill_level": "intermediario",
  "shoulder_risk": "baixo",
  "overhead": false,
  "explosive": false,
  "substitutes": ["goblet_squat", "bulgarian_split_squat"],
  "short_instruction": "Barra nas costas, tronco firme, desça controlado e suba mantendo joelhos alinhados."
}
```

Critérios de aceite:

- Biblioteca lista exercícios.
- É possível identificar por equipamento, padrão e risco de ombro.
- Exercícios overhead/explosivos ficam marcados.
- Substitutos aparecem de forma estruturada.

---

# Fase 3 — Skills e cargas do Gustavo

Criar estrutura para registrar exercícios dominados e cargas conhecidas.

Campos mínimos:

- athlete_id;
- exercise_id;
- skill_status: domina, sabe, aprendendo, evitar;
- last_load_kg;
- best_load_kg;
- best_reps;
- comfort_score;
- shoulder_tolerance;
- notes.

Seed inicial:

```json
[
  {
    "exercise_id": "back_squat",
    "skill_status": "domina",
    "best_reps": 10,
    "best_load_description": "30 kg de cada lado",
    "notes": "Consegue fazer 10 repetições facilmente."
  },
  {
    "exercise_id": "pullup_pronated",
    "skill_status": "domina",
    "best_reps": 10,
    "notes": "Consegue 10 repetições."
  },
  {
    "exercise_id": "pullup_supinated",
    "skill_status": "domina",
    "best_reps": 10,
    "notes": "Consegue 10 repetições."
  },
  {
    "exercise_id": "pushup",
    "skill_status": "domina",
    "best_reps": 30,
    "notes": "Consegue até 30 flexões."
  },
  {
    "exercise_id": "barbell_curl",
    "skill_status": "sabe",
    "notes": "Sabe fazer rosca direta com barra reta."
  },
  {
    "exercise_id": "ez_bar_curl",
    "skill_status": "sabe",
    "notes": "Sabe fazer rosca direta com barra W."
  },
  {
    "exercise_id": "romanian_deadlift",
    "skill_status": "sabe",
    "notes": "Conhecido como stiff / levantamento terra romeno."
  }
]
```

Critérios de aceite:

- Existe tela/seção para skills e cargas.
- Dados iniciais do Gustavo estão cadastrados.
- O app consegue usar skills para evitar exercícios desconhecidos.
- O app exibe carga/reps de referência.

---

# Fase 4 — Melhorar execução do treino

Adicionar por série:

- reps realizadas;
- carga usada;
- RPE/dificuldade de 1 a 10;
- dor no ombro de 0 a 10;
- dor lombar de 0 a 10;
- qualidade técnica: boa, média, ruim;
- observação.

Adicionar botões rápidos:

- Concluí a série;
- Muito fácil;
- Muito pesado;
- Dor no ombro;
- Trocar exercício.

Comportamento:

- Se clicar em dor no ombro, registrar dor e sugerir substituto seguro.
- Se exercício atual for overhead/explosivo e o perfil tiver ombro sensível, mostrar alerta forte ou bloquear.
- Se banco = false, não sugerir supino com banco.

Critérios de aceite:

- Logs salvam RPE, dor no ombro, dor lombar e técnica.
- Tela funciona bem no celular.
- Cronômetro e descanso continuam funcionando.
- Não quebra treinos existentes.
- Há resumo básico ao final.

---

# Fase 5 — Treino full body personalizado do Gustavo

Criar treino oficial:

`barone_fullbody_ombro_60min`

Estrutura:

- Aquecimento: 10 min.
- Principal: 40 min.
- Desacelerar: 5 min.
- Alongamento: 5 min.

Base:

Bloco A:

- Back squat: 4x6-8, referência 30 kg de cada lado.
- Barra fixa: 4x6-8, prona ou supina.

Bloco B:

- Stiff / terra romeno: 4x8-10.
- Flexão ou floor press: 4x15-25 ou equivalente.

Bloco C:

- Remada curvada ou remada unilateral com kettlebell: 3x8-10.
- Afundo búlgaro no step: 3x8 por perna.

Bloco D:

- Rosca direta barra reta ou W: 2-3x10-12.
- Prancha: 2-3x40s.
- Swing russo: 2-3x12-15, apenas se ombro sem dor; nunca swing americano.

Alongamento:

- posterior;
- glúteo;
- peitoral leve;
- latíssimo/costas;
- respiração final.

Critérios de aceite:

- Treino aparece na lista.
- Treino abre na execução.
- Treino respeita equipamentos.
- Treino contém alertas de ombro.
- Permite registrar carga/RPE/dor/técnica.

---

# Fase 6 — Regras de segurança do ombro

Implementar motor simples:

```js
if (shoulderPain >= 5) {
  // parar exercício atual, sugerir substituto, não progredir carga
}

if (shoulderPain >= 3 && shoulderPain <= 4) {
  // reduzir carga, reduzir amplitude ou trocar exercício
}

if (exercise.overhead && athleteHasSensitiveShoulder) {
  // bloquear ou exigir confirmação forte
}

if (exercise.explosive && exercise.overhead && athleteHasSensitiveShoulder) {
  // bloquear por padrão
}
```

Bloqueados por padrão no modo ombro:

- push_press;
- thruster;
- snatch;
- devil_press;
- american_swing;
- overhead_lunge;
- walking_wall;
- burpee_aggressive.

Critérios de aceite:

- App identifica exercícios de risco.
- App sugere substitutos.
- App não progride carga quando há dor relevante.
- App salva eventos de dor.

---

# Fase 7 — Motor de progressão

Regras:

```js
if (allSetsHitTopReps && rpe <= 8 && shoulderPain <= 2 && technique !== "ruim") {
  suggestIncreaseLoad();
}

if (rpe >= 9 || technique === "ruim") {
  maintainOrReduceLoad();
}

if (shoulderPain >= 3) {
  doNotProgress();
}
```

Critérios de aceite:

- Após salvar treino, app gera sugestão simples para o próximo treino.
- Sugestão explica motivo.
- Sugestão considera carga, reps, RPE, dor e técnica.
- Sugestão aparece no histórico.

---

# Fase 8 — Gerador simples do treino do dia

Tela: Treino de hoje.

Perguntar:

- tempo disponível: 30, 45, 60, 75 min;
- energia: 1 a 10;
- dor no ombro hoje: 0 a 10;
- local: casa, academia, viagem;
- objetivo hoje: força, condicionamento, manutenção, recuperação.

Saída:

- treino recomendado;
- motivo da escolha;
- alertas;
- substitutos;
- botão iniciar treino.

Critérios de aceite:

- Gera recomendação sem IA externa.
- Usa perfil, equipamentos, dor e energia.
- Permite iniciar o treino recomendado.

---

# Fase 9 — Preparar para IA futura

Não implementar OpenAI API agora, a menos que seja simples e isolado.

Criar arquitetura preparada:

- `coachContextBuilder`;
- `rulesValidator`;
- `progressionEngine`;
- `shoulderSafetyRules`;
- `workoutGenerator` determinístico inicial.

Critérios:

- Lógica separada em módulos.
- Futuramente será fácil trocar gerador determinístico por IA.
- IA nunca poderá ignorar regras de segurança.

---

# Requisitos gerais

- Manter estilo atual do projeto.
- Preferir código simples.
- Mobile-first.
- Não adicionar dependências pesadas sem necessidade.
- Não remover funcionalidades existentes.
- Não quebrar treinos do Alberto.
- Criar seeds/migrations quando necessário.
- Atualizar frontend API client se criar rotas.
- Atualizar README.
- Garantir que `npm run build` do frontend funcione.
- Garantir que backend rode com `npm run dev` ou `npm start`.
- Tratar erros básicos de API.
- Validar inputs importantes.
- Não armazenar dados sensíveis desnecessários.

---

# Primeira entrega esperada

Não implemente todas as fases de uma vez.

Faça primeiro:

1. Inspecionar estrutura atual.
2. Criar plano técnico curto no output.
3. Implementar Fase 1 e, se couber com segurança, iniciar Fase 2.
4. Atualizar README.
5. Rodar build/testes disponíveis.
6. Informar arquivos alterados e próximos passos.

Ao final, entregue:

- resumo das mudanças;
- como rodar;
- rotas criadas;
- telas criadas;
- limitações;
- próximos passos recomendados.
