# Implementando CRUD com Portinari

Este projeto foi desenvolvido com o desafio de contruir um CRUD básico utilizando portinari-ui.

### Apresentação

Link da [Apresentação](https://drive.google.com/file/d/10l3WWkuAxJavu8F_EMtrQDVztQHgfxcn/view?usp=sharing)

### Versão correta do angular cli.

> Bash

```
    
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

