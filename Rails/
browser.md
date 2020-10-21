普段は、ターミナルにrails routesをたたき、確認していました。

```
$ rails routes
      Prefix Verb   URI Pattern               Controller#Action
sessions_new GET    /sessions/new(.:format)   sessions#new
        root GET    /                         static_pages#home
        help GET    /help(.:format)           static_pages#help
       about GET    /about(.:format)          static_pages#about
     contact GET    /contact(.:format)        static_pages#contact
      signup GET    /signup(.:format)         users#new
             POST   /signup(.:format)         users#create
       login GET    /login(.:format)          sessions#new
             POST   /login(.:format)          sessions#create
      logout DELETE /logout(.:format)         sessions#destroy
       users GET    /users(.:format)          users#index
             POST   /users(.:format)          users#create
    new_user GET    /users/new(.:format)      users#new
   edit_user GET    /users/:id/edit(.:format) users#edit
        user GET    /users/:id(.:format)      users#show
             PATCH  /users/:id(.:format)      users#update
             PUT    /users/:id(.:format)      users#update
             DELETE /users/:id(.:format)      users#destroy
```

# ブラウザ上で確認する方法

(http://localhost:3000/rails/info/routes)

こちらにアクセスするだけです！

左の列から
-Helper
-HTTP Verb
-Path
-Controller#Action

となっていて、構成はターミナルと変わりません。

こちらの方が見やすいですね！