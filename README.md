# Diário de Viagem

## 1. Estrutura Geral do Site
### Páginas principais:
- Home: Apresentação do site/autor, últimas viagens postadas, destaque para lugares especiais;
- Destinos: Lista de países/regiões visitados. Pode ter filtros (por continente, data, tags etc);
- Postagens: Cada viagem vira um post detalhado com texto, fotos, mapa, e categorias;
- Sobre mim: Breve bio com sua motivação, uma foto e talvez um mapa com os lugares já visitados.

## 2. Layout e Design
### Estilo visual:
- Minimalista e limpo: destaque para fotos e textos;
- Paleta de cores suave ou com tons terrosos (evoca natureza, estrada, cultura);
- Tipografia clara: fonte serifada para títulos (ex: Playfair Display) + sans-serif para corpo (ex: Lato, Open Sans).

### Componentes visuais:
- Header fixo com menu e logo;
- Mapa interativo (usando por exemplo Leaflet.js ou embed do Google Maps) com pins clicáveis para posts;
- Cards de postagens com título, imagem de capa e breve descrição;
- Galerias de fotos nas postagens (pode usar carrossel);
- Linha do tempo (timeline) opcional para mostrar a cronologia das viagens.

## 3. Conteúdo de cada post
### Cada entrada do diário pode ter:
- Título + data
- Localização (com mapa embutido)
- Texto narrativo (o que você fez, viu, sentiu)
- Fotos e vídeos
- Dicas úteis (transporte, hospedagem, comida)
- Curiosidades culturais
- Tag por país/cidade

## 4. Recursos técnicos
- Frontend: HTML + CSS (Tailwind ou Bootstrap) + JS. Ou um framework como Next.js se quiser mais robustez;
- Backend (opcional): Node.js ou um CMS como WordPress/Strapi;
- Banco de dados (opcional): MongoDB ou SQLite;
- Hospedagem: Vercel, Netlify (para site estático) ou um VPS como DigitalOcean (se tiver backend).

## Painel admin (dinâmico):
Você acessa uma URL do tipo seudominio.com/admin.  
Lá, você tem formulários para:
- Criar novos posts;
- Subir fotos;
- Escolher categorias;
- Editar ou excluir posts antigos.
Tudo isso sem tocar no código.

## Criando do zero (usando algo como React + Node.js):
Você constrói seu próprio painel, totalmente customizado.

## Estrutura do Projeto
### 1. Estrutura geral do projeto
```
/diario-de-viagem
├── backend
│   ├── controllers
│   ├── models
│   ├── routes
│   └── server.js
├── frontend
│   ├── public
│   ├── src
│   │   ├── components
│   │   ├── pages
│   │   └── App.jsx
└── README.md
```
### 2. Backend (Node.js + Express + MongoDB)
Funcionalidades básicas:  
- Login para acessar o painel;
- CRUD de postagens (Create, Read, Update, Delete);
- Upload de imagens (via multer, por exemplo);
- Armazenamento no MongoDB (ou SQLite se preferir algo local);
**Exemplo de Rota:**
```javascript
// POST /api/posts
router.post('/', authenticate, async (req, res) => {
  const { title, content, location, date } = req.body;
  const post = new Post({ title, content, location, date });
  await post.save();
  res.status(201).json(post);
});
```

### 3. Frontend (React)
**Principais páginas do admin:**
- /admin/login: tela de login
- /admin/dashboard: lista de posts, botão de novo post
- /admin/new: formulário para novo post
- /admin/edit/:id: editar postagem
**Funcionalidades extras:**
- Preview em tempo real
-Upload de imagem com preview
- Modo rascunho/publicado

### 4. Segurança básica
- Autenticação com JWT (token salvo em localStorage);
- Proteção de rotas admin;
- Validação de entrada.

### 5. Banco de dados (MongoDB)
**Modelo de Post:**
```javascript
{
  title: String,
  content: String,
  location: String,
  date: Date,
  images: [String],
  tags: [String],
  createdAt: Date
}
```

### Extras pra deixar mais legal:
- Mapa com pins dos posts usando Leaflet;
- Tema claro/escuro;
- Deploy no Vercel (frontend) e Render/Heroku (backend);
- Integração com Git para versionar seu diário.

## Conteúdo do Site
### 1. Estrutura das postagens
**Cada post deve ter:**
- title: Título da entrada;
-date: Data exata da experiência (ex: 2023-04-10);
- location: Cidade/país;
- content: Texto e imagens;
- tags: Ex: #chuva, #praia, #aventura;

### 2. Como filtrar por data
**a) Por calendário**
- Um pequeno calendário interativo onde você clica no dia e vê os posts daquele dia.
- Biblioteca recomendada: react-calendar
- 
**b) Por linha do tempo**
- Exibir os posts em ordem cronológica, com agrupamento por mês/ano.
- Pode usar um componente como:
  - react-vertical-timeline-component
  - ou montar o seu: mostrar um marcador de mês e os posts abaixo

**c) Por busca ou filtro**
- Adicione um formulário de busca por data:
  - Intervalo (de X até Y)
  - Seletor de ano/mês
- Backend deve permitir query tipo:
```javascript
// Exemplo: pegar posts de abril de 2023
Post.find({
  date: {
    $gte: new Date('2023-04-01'),
    $lt: new Date('2023-05-01')
  }
});
```

### 3. Layouts sugeridos para esse tipo de conteúdo
**Página de diário**
- Cabeçalho com mês/ano
- Cards dos posts com:
  - Data
  - Local
  - Miniatura
  - Trecho do conteúdo
**Navegação lateral**
- Um menu colapsável por ano > mês
- Exemplo:
```
2023
  - Abril (5 posts)
  - Março (3 posts)
2022
  - Dezembro (7 posts)
```
**Página de um post individual**
Mostra:  
- Data + Localização
- Conteúdo completo
- Galeria de imagens
- Botão: "Ver mais posts desta viagem / deste mês"

## Filtrando por Localidade
### 1. Estrutura dos dados
**Cada post precisa guardar informações como:**
```javascript
{
  title: "Explorando Bruges",
  date: "2023-06-10",
  location: {
    country: "Bélgica",
    region: "Flandres Ocidental",
    city: "Bruges"
  },
  ...
}
```
Você pode usar esses dados para criar filtros por:  
- País
- Região/estado
- Cidade

### 2. Como implementar o filtro
**a) Filtro por menu suspenso**
- Ex: 3 selects encadeados:
  - País → Região → Cidade
- Escolher um país carrega as regiões, e assim por diante.
- Exemplo (React + backend):
```javascript
// Filtrar posts da Bélgica > Flandres Ocidental > Bruges
Post.find({
  'location.country': 'Bélgica',
  'location.region': 'Flandres Ocidental',
  'location.city': 'Bruges'
});
```

**b) Mapa interativo**
- Um mapa com pins das cidades visitadas.
- Ao clicar em uma cidade, mostrar os posts relacionados.
- Pode usar:
  - Leaflet.js
  - Mapbox
  - Google Maps API

**c) Navegação por lista**
Um painel lateral ou dropdown com:
```
Bélgica
  - Flandres Oriental
    - Gent
    - Sint-Niklaas
  - Flandres Ocidental
    - Bruges
    - Ostende
Holanda
  - Utrecht
    - Amersfoort
```
### 3. Página de localidade
Para cada cidade ou país, você pode ter uma rota dedicada, tipo:  
- /lugares/belgica/bruges
- Mostra todos os posts relacionados àquele local, com:
  - Data
  - Título do post
  - Trecho do conteúdo
  - Mapa com os pontos dos posts

### 4. Extras interessantes
- Tags por tipo de lugar: praia, floresta, cidade grande, vila, montanha etc.
- Posts relacionados: “Você também escreveu sobre outras cidades da Bélgica”.
