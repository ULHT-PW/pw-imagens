# Utilizando os campos ImageField e FileField num modelo 

Para adicionar um campo de imagem/ficheiro a uma classe, e manusear inserir imagens e ficheiros na base de dados através de formulários, deverá fazer os seguintes passos extra:

1. Criar, na root, a pasta `media/app` (em vez de `app` use o nome da aplicaçao que está a usar, ou um nome à sua escolha) para guardar os ficheiros carregados. Na pasta media deve colocar uma pasta com o nome da aplicação (no exemplo em baixo, aplicação app).

```bash
> project
> media/app
> app
db.sqlite3
manage.py
```

2. Em `settings.py` adicionar referencia à pasta criada, com `MEDIA_URL` e `MEDIA_ROOT`:

```Python
MEDIA_URL = '/media/'
MEDIA_ROOT = os.path.join(BASE_DIR, 'media')
```

3. Em `config/urls.py` adicionar, depois de urlpatterns:

```Python
from django.conf import settings
from django.conf.urls.static import static

urlpatterns += static(
      settings.MEDIA_URL,
      document_root=settings.MEDIA_ROOT)
```

4. Em `models.py`, na classe adicionar um atributo com o campo imagem/ficheiro. Deverá especificar para onde devem ser carregados os ficheiros. O django como base a pasta media, neste caso ao especificar `upload_to='app/fotos'`, os ficheiros sendo carregados na pasta `media/app/fotos/`.

```Python
imagem = models.ImageField(upload_to='app/fotos', null=True, blank=True)
ficheiro = models.FileField(upload_to='app/ficheiros', null=True, blank=True)
```

5. No ficheiro `views.py`, onde cria uma instância do formulário, adicionar `request.FILES`:

```Python
form = AppForm(request.POST or None, request.FILES)
```


6. No ficheiro HTML, no elemento `form` adicionar `enctype`:
```Python
<form method="POST" enctype="multipart/form-data">
```

7. Para inserir no HTML uma imagem associada a um objeto, utilize o campo `url` de `imagem`:

```HTML
<img src="{{ app.imagem.url }}">
```

8. Para inserir o link para um ficheiro associado a um objeto, utilize os campos `url` e `name` de `ficheiro`:

```HTML
Ficheiro: <a href="{{ app.ficheiro.url }}">{{ app.ficheiro.name }}</a>
```
