# Escala HMSA — Sistema de Gestão de Acadêmicos

## Arquivos do sistema
- `index.html` — app dos acadêmicos (escala, presença, troca)
- `admin.html` — painel da secretaria (protegido por senha)
- `README.md` — este arquivo com instruções

---

## PASSO 1 — Publicar no GitHub Pages

1. Acesse https://github.com e faça login com @sgancz-dotcom
2. Clique em **"New repository"**
3. Nome do repositório: `escala-hmsa`
4. Marque como **Public**
5. Clique em **"Create repository"**
6. Na próxima tela, clique em **"uploading an existing file"**
7. Arraste os 3 arquivos: `index.html`, `admin.html`, `README.md`
8. Clique em **"Commit changes"**
9. Vá em **Settings → Pages → Source: Deploy from a branch → Branch: main**
10. Aguarde 2 minutos e acesse:
    - Acadêmicos: https://sgancz-dotcom.github.io/escala-hmsa
    - Admin: https://sgancz-dotcom.github.io/escala-hmsa/admin.html

---

## PASSO 2 — Criar Google Forms

### Form 1: Confirmação de Presença
1. Acesse https://forms.google.com → "Em branco"
2. Título: **"Confirmação de Presença — HMSA"**
3. Adicione os campos:
   - Pergunta 1: "Seu nome completo" (tipo: texto)
   - Pergunta 2: "Turno" (tipo: múltipla escolha → Dia / Noite)
   - Pergunta 3: "Data do plantão" (tipo: data)
4. Clique em ⚙️ Configurações → Respostas → ative "Coletar endereços de e-mail: Não"
5. Clique em **Enviar → Link** e copie a URL
6. Para pegar os entry IDs:
   - Clique em "Visualizar" (ícone do olho)
   - Inspecione o formulário (F12 → Elements)
   - Procure por `entry.XXXXXXXXX` em cada campo
   - Anote os 3 entry IDs

### Form 2: Solicitação de Troca
1. Novo formulário → Título: **"Solicitação de Troca de Plantão — HMSA"**
2. Campos:
   - "Quem solicita a troca" (texto)
   - "Dia do seu plantão" (múltipla escolha: Segunda/Terça/Quarta/Quinta/Sexta/Sábado/Domingo)
   - "Turno do seu plantão" (múltipla escolha: Dia / Noite)
   - "Com quem quer trocar" (texto)
   - "Dia do plantão do colega" (múltipla escolha: mesmas opções)
   - "Turno do plantão do colega" (múltipla escolha: Dia / Noite)
   - "Motivo da troca" (parágrafo)
   - "Carga horária do solicitante" (texto — preenchido automaticamente pelo app)
   - "Carga horária do parceiro" (texto — preenchido automaticamente pelo app)
3. Anote os 9 entry IDs

---

## PASSO 3 — Configurar os IDs no código

### No arquivo `index.html`, localize a seção:
```javascript
const FORM_PRESENCA = "https://docs.google.com/forms/d/e/COLOQUE_ID_DO_FORM_PRESENCA_AQUI/formResponse";
const FORM_TROCA    = "https://docs.google.com/forms/d/e/COLOQUE_ID_DO_FORM_TROCA_AQUI/formResponse";

const FIELDS_PRESENCA = { nome: "entry.000000001", turno: "entry.000000002", data: "entry.000000003" };
const FIELDS_TROCA = {
  solicitante: "entry.000000004",
  ...
};
```

Substitua:
- `COLOQUE_ID_DO_FORM_PRESENCA_AQUI` pelo ID real do Form 1
- `COLOQUE_ID_DO_FORM_TROCA_AQUI` pelo ID real do Form 2
- Todos os `entry.000000XXX` pelos entry IDs reais de cada campo

---

## PASSO 4 — Configurar a senha do painel admin

No arquivo `admin.html`, localize:
```javascript
const SENHA_ADMIN = "hmsa2026";
```

Troque `hmsa2026` por uma senha segura antes de publicar.

---

## PASSO 5 — Vincular respostas dos Forms ao Google Sheets

1. Abra cada Form → aba **"Respostas"**
2. Clique no ícone do Sheets (verde) → "Criar planilha"
3. As respostas chegarão automaticamente nas abas "Presenças" e "Trocas"
4. No `admin.html`, atualize os links:
```javascript
const SHEETS_URL = "https://docs.google.com/spreadsheets/d/SEU_SHEETS_ID/edit";
```

---

## Como atualizar a escala

Quando houver mudança de escala:
1. Abra `index.html` em um editor de texto (Bloco de Notas, TextEdit, VSCode)
2. Localize o bloco `const ESCALA = {`
3. Edite os nomes nos dias correspondentes
4. Salve e faça upload novamente no GitHub (o link não muda)

---

## FUTURO — Análise automática por Claude API

Quando ativado, o sistema irá:
- Ler a solicitação de troca automaticamente
- Verificar carga horária dos dois acadêmicos
- Checar se o plantão ficará descoberto
- Verificar histórico de trocas do mês
- Emitir parecer com recomendação de aprovação ou rejeição
- Notificar a secretaria apenas para decisão final

**Custo estimado:** R$ 2–5/mês para o volume atual de trocas.
Para ativar, entre em contato ou veja: https://console.anthropic.com

---

## Contato técnico
Sistema desenvolvido com assistência de Claude (Anthropic).
Dúvidas técnicas: consulte claude.ai
