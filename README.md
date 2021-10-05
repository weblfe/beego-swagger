# beego-swagger

beego middleware to automatically generate RESTful API documentation with Swagger 2.0.


## Usage

### Start using it
1. Add comments to your API source code, [See Declarative Comments Format](https://swaggo.github.io/swaggo.io/declarative_comments_format/).
2. Download [Swag](https://github.com/swaggo/swag) for Go by using:
```sh
$ go get -u github.com/swaggo/swag/cmd/swag  
$ go get -u github.com/weblfe/swag2/cmd/swag # swag2 support write comment in interface and show scan file path
```

3. Run the [Swag](https://github.com/swaggo/swag) in your Go project root folder which contains `main.go` file, [Swag](https://github.com/swaggo/swag) will parse comments and generate required files(`docs` folder and `docs/doc.go`).
```sh
$ swag init
```
4. Download [beego-swagger](https://github.com/arsmn/beego-swagger) by using:
```sh
$ go get -u github.com/weblfe/beego-swagger
```
And import following in your code:

```go
import "github.com/weblfe/beego-swagger" // beego-swagger middleware

```

### Canonical example:

```go
package main

import (
	swagger "github.com/weblfe/beego-swagger"
	"github.com/astaxie/beego"

	// docs are generated by Swag CLI, you have to import them.
	_ "github.com/weblfe/beego-swagger/example/docs"
)

// @title Fiber Example API
// @version 1.0
// @description This is a sample swagger for Fiber
// @termsOfService http://swagger.io/terms/
// @contact.name API Support
// @contact.email beego@swagger.io
// @license.name Apache 2.0
// @license.url http://www.apache.org/licenses/LICENSE-2.0.html
// @host localhost:8080
// @BasePath /
func main() {
	

	beego.Get("/swagger/*", swagger.Handler) // default

	beego.Get("/swagger/*", swagger.New(swagger.Config{ // custom
		URL: "http://example.com/doc.json",
		DeepLinking: false,
		// Expand ("list") or Collapse ("none") tag groups by default
		DocExpansion: "none",
		// Prefill OAuth ClientId on Authorize popup
		OAuth: &swagger.OAuthConfig{
			AppName:  "OAuth Provider",
			ClientId: "21bb4edc-05a7-4afc-86f1-2e151e4ba6e2",
		},
		// Ability to change OAuth2 redirect uri location
		OAuth2RedirectUrl: "http://localhost:8080/swagger/oauth2-redirect.html",
	}))

	app.Listen(":8080")
}
```
5. Run it, and browser to http://localhost:8080/swagger, you can see Swagger 2.0 Api documents.

<p align="center"><img src="example/docs/swagger-ui.png"></p>
