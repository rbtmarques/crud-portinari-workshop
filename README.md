# Implementando CRUD com Portinari

Este projeto foi desenvolvido com o desafio de contruir um CRUD básico utilizando portinari-ui.

### Apresentação

Link da [Apresentação](https://drive.google.com/file/d/10l3WWkuAxJavu8F_EMtrQDVztQHgfxcn/view?usp=sharing)

### Versão correta do angular cli.

> Bash
```
  npm install -g @angular/cli@8.0.0
```

### Inicializando um projeto angular novo.

> Bash
```
 ng new crud-portinari --skipInstall --routing --style=css
```

### Verificando as dependências 
[Documentação](https://portinari.io/guides/getting-started)

```
 "dependencies": {
    "@angular/animations": "~8.0.0",
    "@angular/common": "~8.0.0",
    "@angular/compiler": "~8.0.0",
    "@angular/core": "~8.0.0",
    "@angular/forms": "~8.0.0",
    "@angular/platform-browser": "~8.0.0",
    "@angular/platform-browser-dynamic": "~8.0.0",
    "@angular/platform-server": "~8.0.0",
    "@angular/router": "~8.0.0",
    "rxjs": "~6.4.0",
    "zone.js": "~0.9.1"
    ...
  }
```

### Instalando o npm
> Bash
```
 npm install
```

### Adicionando portinari ao projeto.
> Bash
```
 ng add @portinari/portinari-ui
```

### Configuração dos módulos do projeto

### Criação do módulo de Shared.

> Bash
```
 ng g m shared
```

### shared.module.ts

> TS
```
  @NgModule({
    imports: [
      CommonModule,
      FormsModule,
      PoModule
    ],
    exports: [
      CommonModule,
      FormsModule,
      PoModule
    ]
  })
```

### app.module.ts

> TS
```
  imports: [
    BrowserModule,
    AppRoutingModule,
    SharedModule,
    RouterModule.forRoot([])
  ]
```

### Criação do módulo de pessoas.

> Bash
```
 ng g m people --routing
```

### Criação do componente de Lista

> Bash
```
 ng g c people\people-list
```

### Criação do componente de Visualização.

> Bash
```
 ng g c people\people-view
```

### Criação do componente de Formulário de Edição.

> Bash
```
 ng g c people\people-form
```

### people.module.ts

> TS
```
  imports: [
    BrowserModule,
    AppRoutingModule,
    SharedModule,
    RouterModule.forRoot([])
  ]
```

### people-rounting.module

> TS
```
const routes: Routes = [
  { path: '', component: PeopleListComponent },
  { path: 'view/:id', component: PeopleViewComponent },
  { path: 'edit/:id', component: PeopleFormComponent },
  { path: 'new', component: PeopleFormComponent }
];
```

### app-routing.module.ts

> TS
```
const routes: Routes = [
  {
    path: 'people',
    loadChildren: () => import('../app/people/people.module').then(m => m.PeopleModule)
  },
  { path: '', redirectTo: '/people', pathMatch: 'full'}
];
```

### app.component.ts

> TS
```
  { label: 'Pessoas', link: '/people' }
```


### people-list.component

> HTML
```
<thf-page-dynamic-table
  t-title="Pessoas"
  t-service-api="https://thf.totvs.com.br/sample/api/people"
  [t-actions]="actions"
  [t-fields]="fields">
</thf-page-dynamic-table>
```

> TS
```
  readonly actions: ThfPageDynamicTableActions = {
    detail: 'people/view/:id',
    edit: 'people/edit/:id',
    new: 'people/new',
    remove: true
  };

  readonly fields: Array<ThfPageDynamicTableField> = [
    { property: 'id', key: true },
    { property: 'name', label: 'Nome' },
    { property: 'birthdate', label: 'Data de nascimento', type: 'date' }
  ];

```

### people-view.component

> HTML
```
<thf-page-dynamic-detail
  t-service-api="https://thf.totvs.com.br/sample/api/people"
  [t-title]="title"
  [t-actions]="actions"
  [t-fields]="fields">
</thf-page-dynamic-detail>
```

> TS
```
  title = 'Visualizando';

  readonly actions: ThfPageDynamicDetailActions = {
    back: '/',
    edit: 'people/edit/:id',
    remove: '/'
  };

  readonly fields: Array<ThfPageDynamicDetailField> = [
    { property: 'id', gridColumns: 2, key: true, divider: 'Dados pessoais' },
    { property: 'name', label: 'Nome', gridXlColumns: 4, gridLgColumns: 4 },
    { property: 'birthdate', type: 'date', label: 'Data de aniversário', gridXlColumns: 4, gridLgColumns: 4 },
    { property: 'genre', tag: true, label: 'Gênero', gridColumns: 2, gridSmColumns: 6 },
    { property: 'street', divider: 'Endereço', label: 'Rua' },
    { property: 'city', label: 'Cidade' },
    { property: 'country', label: 'País' }
  ];

  constructor(private activatedRoute: ActivatedRoute) { }

  ngOnInit() {
    this.activatedRoute.params.subscribe(params => {
      this.title = params.id ? `Vizualizando Pessoa ${params.id}` : 'Visualizando';
    });
  }
```

### people-form.component

> HTML
```
<thf-page-dynamic-edit
  t-service-api="https://thf.totvs.com.br/sample/api/people"
  [t-title]="title"
  [t-actions]="actions"
  [t-fields]="fields">
</thf-page-dynamic-edit>
```

> TS
```
title = 'Nova pessoa';

actions: ThfPageDynamicEditActions = {
  cancel: '/',
  save: '/'
};

fields: Array<ThfPageDynamicEditField> = [
  { property: 'id', key: true, visible: false },
  { label: 'Nome', property: 'name', divider: 'Dados pessoais' },
  { label: 'Data de nascimento', property: 'birthdate', type: 'date' },
  { label: 'Gênero', property: 'genre', gridXlColumns: 4, options: [
    { value: 'feminino', label: 'Feminino' },
    { value: 'masculino', label: 'Masculino' }
  ]},
  { label: 'Rua', property: 'street', divider: 'Endereço' },
  { label: 'Cidade', property: 'city' },
  { label: 'País', property: 'country' }
];

constructor(private activatedRoute: ActivatedRoute) { }

ngOnInit() {
  this.activatedRoute.params.subscribe(params => {
    this.title = params.id ? 'Editando' : 'Nova pessoa';
  });
}

```

