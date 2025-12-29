## i18next com React 18

O **i18next** é uma biblioteca de internacionalização que permite traduzir textos em aplicações React de forma organizada. Ele separa o conteúdo da interface do código, suporta múltiplos idiomas e objetos complexos, e funciona muito bem com **React 18** e **Next.js**.

-----

### Vantagens do i18next

* **Separação** de código e conteúdo (limpeza e organização).
* Fácil de **adicionar novos idiomas**.
* Suporte a **arrays e objetos** diretamente do JSON.
* **Atualização dinâmica** de idioma (sem recarregar a página).
* Integração suave com **React 18**, **Next.js** e outros frameworks.

-----

### Instalando i18next

No projeto React 18:

```bash
npm install i18next react-i18next
```

Se estiver usando Next.js, também é recomendado:

```bash
npm install next-i18next
```

-----

### Estrutura de Arquivos de Tradução

A estrutura organiza os arquivos JSON por idioma:

```
i18n/
  translations/
    en/
      features.json
      header.json
      footer.json
    pt/
      features.json
      header.json
      footer.json
```

#### Exemplo `features.json` (EN)

```json
{
  "sectionLabel": "Features",
  "title": "Awesome <span>Features</span>",
  "subtitle": "Check out what makes our app special",
  "cards": [
    { "title": "Templates", "description": "Create templates easily" },
    { "title": "AI Integration", "description": "Automatic content filling" }
  ],
  "highlights": {
    "lessManual": "Less manual work",
    "faster": "Faster workflow",
    "accuracy": "Higher accuracy"
  }
}
```

#### Exemplo `features.json` (PT)

```json
{
  "sectionLabel": "Funcionalidades",
  "title": "Funcionalidades <span>Incríveis</span>",
  "subtitle": "Veja o que torna nosso app especial",
  "cards": [
    { "title": "Modelos", "description": "Crie modelos facilmente" },
    { "title": "Integração com IA", "description": "Preenchimento automático" }
  ],
  "highlights": {
    "lessManual": "Menos trabalho manual",
    "faster": "Fluxo mais rápido",
    "accuracy": "Maior precisão"
  }
}
```

-----

### Inicializando o i18next (i18n.ts)

O arquivo de configuração importa os recursos e inicializa a biblioteca:

```typescript
import i18n from "i18next"
import { initReactI18next } from "react-i18next"

// Importação dos arquivos de tradução
import en from "./translations/en/features.json"
import pt from "./translations/pt/features.json"

i18n
  .use(initReactI18next) // Integração com React
  .init({
    // Recurso de tradução, mapeando idioma e namespace (aqui, 'translation')
    resources: {
      en: { translation: en },
      pt: { translation: pt },
    },
    lng: "en", // Idioma padrão inicial
    fallbackLng: "en", //  Idioma de fallback (se a chave não for encontrada)
    interpolation: {
      escapeValue: false, // Permite usar HTML no JSON
    },
  })

export default i18n
```

-----

### Usando no React

#### Hook Básico para Strings

Use o hook `useTranslation` para acessar a função `t()` (translate):

```javascript
import { useTranslation } from "react-i18next"

function Component() {
  const { t } = useTranslation()

  // Traduz a chave "sectionLabel"
  return <h1>{t("sectionLabel")}</h1>
}
```

#### Arrays e Objetos

Para traduzir arrays ou objetos completos do JSON, use a opção `returnObjects: true`:

```javascript
const cards = t("cards", { returnObjects: true }) // 'cards' é um array no JSON

cards.map((card, i) => (
  <div key={i}>
    <h3>{card.title}</h3> {/* Acessa a propriedade 'title' do objeto */}
    <p>{card.description}</p>
  </div>
))
```

#### HTML dentro do JSON

Use `dangerouslySetInnerHTML` em combinação com `interpolation.escapeValue: false` (configurado acima) para renderizar tags HTML embutidas (como o `<span>` no título):

```javascript
<h2 dangerouslySetInnerHTML={{ __html: t("title") }} />
```

#### Mudando de Idioma Dinamicamente

Você pode alterar o idioma em tempo de execução usando o método `changeLanguage`:

```javascript
import i18n from "./i18n" // Importe sua instância configurada

i18n.changeLanguage("pt") // Muda para português
```

-----

### Observações

* **returnObjects: true** é essencial quando você precisa obter **arrays** ou **objetos** completos diretamente do JSON de tradução.
* É possível incorporar **HTML** nos textos para formatar títulos ou palavras específicas.
* É prática comum criar um **hook próprio** ou um componente de **seletor de idioma** que utiliza o i18n.changeLanguage() e salva a preferência do usuário (ex: no localStorage).
* O i18next funciona perfeitamente com **React 18** e **Next.js**, suportando inclusive componentes com **Server-Side Rendering (SSR)**.
* É ideal para aplicações com **múltiplos idiomas** e **atualização frequente de textos**, promovendo uma manutenção simples e centralizada.
