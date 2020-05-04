# Irinc Script

**Conceitos Básicos:**
 - **Comando :script**
 - ***Eventos Assíncronos*** 
 - ***Eventos Síncronos***

**Entidades:**
 - ***ScriptEntity***
 - ***ScriptFurni***
 - ***FakeItem***

**Objetos de manipulação:**
 - ***Room***
 - ***Engine***
 - ***Delay***
 - ***Commands***
 - ***Faker***

**Persistência** 
 - **GlobalStorage**
 - **RoomStorage**

## Comando :script
O Comando :script da acesso total a todos os controles relacionados a execução de scripts no quarto, sendo a utilização: 

    :script -> Abre a o editor de scripts;
    :script status -> Exibe se o quarto está ou não com os scripts ativos;
    :script enable -> Habilita a execução de scripts no quarto;
    :script disable -> Desabilita a execução de scripts no quarto;
    :script log -> Exibe os logs de execução ;
    :script tool -> Habilita/desabilita a ferramenta de seleção de móbis.

## Eventos Assíncronos
Os eventos assíncronos são chamados  pelo **emulador** no momento de execução do quarto, o seu bloqueio não afeta a jogabilidade no quarto.
   ```javascript 
	//Evento chamado quando o quarto é carregado
	function onLoad(){}
	
	//Evento é chamado quando o quarto é descarregado
	function onDispose(){}
	
	//Evento é acionado todas as vezes que um usuário entra no quarto
	function onUserJoin(entity){
		//entidade:ScriptEntity (user que entrou)
	}
	
	//Evento é acionado todas as vezes que um usuário sai do quarto
	function onUserLeave(userId, userName){
		//userId: number e userName:String
	}
	
	//Evento é acionado quando o usuário pisa em um mobi (qualquer quer seja)
	function onStepOn(entity, furni){
		//entidade:ScriptEntity (causador) e o furni:ScriptFurni (afetado).
	}
	
	//Evento é acionado quando o usuário sai do um mobi (qualquer quer seja)
	function onStepOff(entity, furni){
		//entidade:ScriptEntity (causador) e o furni:ScriptFurni (afetado).
	}
	
	//Evento é acionado todas as vezez que o usuário fala no quarto
	function onSay(entity, message){
		//entidade:ScriptEntity (causador) e message:String
	}
	
	//Evento é acionado todas as vezes que o usuário interage (Usa) com algum mobi (qualquer que seja)
	function onInteract(entity, furni){
		//entidade:ScriptEntity (causador) e o furni:ScriptFurni (afetado).
	}
	
	//O quarto possui um loop infinito onde há uma interação a cada 500ms 
	//O onTick é chamado todas as vezes que esse loop é interado.
	function onTick(){}
```
## Eventos Síncronos
Os eventos síncronos são invocados da mesma maneira dos assíncronos, porem só devem ser usados em **casos excepcionais** onde não há outra forma de resolver o problema.

> **OBS:** O mal uso pode causar crash e lag no quarto!

```javascript
	//Evento é acionado quando o usuário pisa em um mobi (qualquer quer seja)
	function onStepOnSync(entity, furni){
		//entidade:ScriptEntity (causador) e o furni:ScriptFurni (afetado).
	}
	
	//Evento é acionado quando o usuário sai do um mobi (qualquer quer seja)
	function onStepOffSync(entity, furni){
		//entidade:ScriptEntity (causador) e o furni:ScriptFurni (afetado).
	}
	
	//Evento é acionado todas as vezes que um usuário entra no quarto
	function onUserJoinSync(entity){
		//entidade:ScriptEntity (user que entrou)
	}
	
	//Evento é acionado todas as vezes que um usuário sai do quarto
	function onUserLeaveSync(userId, userName){
		//userId: number e userName:String
	}
```  

## Entidade: ScriptEntity
Esta entidade representa: Usuários, Bots e Pets 
### Como identificar o tipo da entidade?
```javascript
//Retorna true se a entidade for um jogador
entity.isPlayer()
//Retorna true se a entidade for um BOT
entity.isBot()
//Retorna true se a entidade for um PET
entity.isPet()
```
### Funções e métodos da entidade Player
```javascript
//Teleporta usuário para as coordenadas
player.teleport(x,y,z)
//Teleporta para o furni
player.teleportToFurni(furni)
//Anda até as coordenadas
player.walk(x,y)
//Anda até o furni
player.walkToFurni(furni)
// Kicka o usuário do quarto
player.kick()
//Envia o usuário para o quarto selecionado
player.gotoRoom(roomId)
//Faz o usuário falar no quarto
player.say(message)
//Faz o usuário falar no quarto, caso shout (true) grita
player.say(message, shout)
//Faz o usuário falar com determinada bolha
player.say(message, shout, bubbleId)
//Envia um sussuro para o usuário 
player.message(message)
//Envia uma notifiação para o usuário
player.notification(icon, message)
//Da emblema ao usuário
player.giveBadge(badge)
//Remove o emblema ao usuário
player.removeBadge(badge)
//Remove o handitem do usuário
player.removeHandItem()
//Retorna true caso o usuário esteja no furni
player.inFurni(furni)
//Seta efeito no usuário
player.setEffect(effectId)
//Seta handitem no usuário
player.setHandItem(handItemId)
//Seta dança no usuário
player.setDance(danceId /*(0..4)*/)
// true  para poder andar false para parar.
player.setCanWalk(bool)
//Seta visual
player.setFigure(figure)
//Retorna true caso o usuário possua um rank >= ao escolhido.
player.hasRank(rank)
player.getFigure()
//Retorna missão do usuário.
player.getMotto()
//Retorna dança atual (0 para parado).
player.getDance()
//Pega o efeito atual do usuário (0 para nenhum )
player.getEffect()
//Pega o handitem atual do usuário
player.getHandItem()
//Retorna true caso o usuário possa andar pelo quarto
player.canWalk()
//Retorna o nome de usuário
player.getUsername()
//Retorna o ID do usuário
player.getId()
//Coordenas basicas
player.getX()
player.getY()
player.getZ()
//Retorna a rotação do usuário
player.getR()
	
```
### Funções e métodos da entidade BOT 
```javascript
//Teleporta usuário para as coordenadas
bot.teleport(x,y,z)
//Teleporta para o furni
bot.teleportToFurni(furni)
//Anda até as coordenadas
bot.walk(x,y)
//Anda até o furni
bot.walkToFurni(furni)
//Faz o bot falar no quarto
bot.say(message)
//Faz o bot falar no quarto, caso shout (true) grita
bot.say(message, shout)
//Faz o usuário falar com determinada bolha
bot.say(message, shout, bubbleId)
//Remove o handitem do usuário
bot.removeHandItem()
//Retorna true caso o bot esteja no furni
bot.inFurni(furni)
//Seta efeito no bot
bot.setEffect(effectId)
//Seta handitem no bot
bot.setHandItem(handItemId)
//Seta dança no bot
bot.setDance(danceId /*(0..4)*/)
// true  para poder andar false para parar.
bot.setCanWalk(bool)
//Seta visual
bot.setFigure(figure)
//Seta missão no bot
bot.setMotto(motto)
//Set username do bot
bot.setUsername(username);
//Retorna o figure d bot
bot.getFigure()
//Retorna missão do bot.
bot.getMotto()
//Retorna dança atual (0 para parado).
bot.getDance()
//Pega o efeito atual do bot(0 para nenhum )
bot.getEffect()
//Pega o handitem atual do bot
bot.getHandItem()
//Retorna true caso o usuário possa andar pelo quarto
bot.canWalk()
//Retorna o nome de usuário
bot.getUsername()
//Retorna o ID do usuário
bot.getId()
//Coordenas basicas
bot.getX()
bot.getY()
bot.getZ()
//Retorna a rotação do usuário
bot.getR()
	
```
### Funções e métodos da entidade PET
```javascript
//Teleporta usuário para as coordenadas
pet.teleport(x,y,z)
//Teleporta para o furni
pet.teleportToFurni(furni)
//Anda até as coordenadas
pet.walk(x,y)
//Anda até o furni
pet.walkToFurni(furni)
//Remove o handitem do usuário
pet.removeHandItem()
//Retorna true caso o bot esteja no furni
pet.inFurni(furni)
// true  para poder andar false para parar.
pet.setCanWalk(bool)
//Retorna true caso o usuário possa andar pelo quarto
pet.canWalk()
//Retorna o nome de usuário
pet.getUsername()
//Retorna o ID do usuário
pet.getId()
//Coordenas basicas
pet.getX()
pet.getY()
pet.getZ()
//Retorna a rotação do usuário
pet.getR()
	
```
## Entidade: ScriptFurni
Entidade responsável por representar qualquer furni no quarto.
### Funções e métodos:
```javascript
//Move furni para a cordenanda
furni.move(x,y,z,rotation)
//Retorna uma array com todas as entidades em cima do mobi
furni.getEntities()
//Esconde o furni
furni.hide()
//Mostra o furni (caso ele tenha sido escondido)
furni.show()
//Retorna true se tiver 1 ou mais entidades no mobi
furni.hasEntities()
//Interage como o wired de estado
furni.toggleState()
//Seta o estado
furni.setState(state)
//Retorna o estado atual do mobi
furni.getState()	
//Retorna o ID do mobi
furni.getId()
//Coordenas basicas
furni.getX()
furni.getY()
furni.getZ()
```
## Entidade: FakeItem
Entidade gerada pelo **Faker** onde representa uma um furni visual
### Funções e métodos:
```javascript
//Move furni para a cordenanda
furni.move(x,y,z,rotation)
//Seta o estado
furni.setState(state)
//Retorna o estado atual do mobi
furni.getState()	
//Retorna o ID do mobi
furni.getId()
```
## Objetos de manipulação
Esses objetos são injetados assim que o script é iniciado, e são responsáveis pela manipulação gera do quarto

## Objeto: Room
O objeto **Room** é responsável pela manipulação do quarto em geral
### Funções e métodos:
```javascript
//Retorna o ScriptFurni correspondente ao ID
Room.getFurniById(id)
//Retorna um array de ScriptFurni correspondente ao tile
Room.getFurniByTile(x,y)
//Retorna ScriptEntity do tipo player
Room.getPlayerByName(name)
//Retorna ScriptEntity do tipo bot
Room.getBotByName(botName)
//Retorna ScriptEntity do tipo player
Room.getPlayerById(id)
//Seta diagonal
Room.setDiagonal(bool)
//Estado da diagonal
Room.getDiagonal()
//Seta atravessar
Room.setWalkThrough(bool)
//Estado do atravessar
Room.getWalkThrough()
//Seta o nome do quarto
Room.setName(roomName)
//Retorna o nome do quarto
Room.getName()
//Muda acessibilidade do quarto para Aberto
Room.open()
//Muda acessibilidade do quarto para campainha
Room.setDoorbell()
//Muda acessibilidade do quarto para senha
Room.setPassword(password)
//Retorna o ID do quarto
Room.getId()
//Retorna o ID do dono
Room.ownerId()
//Retorna o username do dono
Room.getOwnerUsername();
//Retorna a quantidade de usuário no quarto
Room.userCount()
//Envia alerta a todos do quarto
Room.alert(message)
//Envia notificação a todos do quarto
Room.notification(icon, message)
```
## Objeto: Engine
O objeto **Engine** é responsável pelo controle da instancia do script
### Funções e métodos:
```javascript
//Envia menssagem de debug
Engine.d(messgae)
//Envia erro para o log
Engine.e(message)
```
## Objeto: Delay
O objeto **Delay** implementa as funções similares a o do javascript de navegador

> O tempo é medido em **TICKS** e cada tick dura 500ms

### Uso:
```javascript
//Agenda uma tarefa a ser executada no tempo definido
var deleyedExec = Delay.wait(function(){
}, ticks)
//Executa a tarefa repetidamente no intervalo definido
var interval = Delay.interval(function(){
}, ticks)

//Converte Segundos para ticks
Delay.seconds(ticks)
//Cancela a execução de uma Delay.wait ou de uma Delay.interval
Delay.cancel(delayTask)
```

## Objeto: Commands
O objeto **Commands** implementa as funções para facilitar a execução de comandos

### Uso:
```javascript

//Executa apenas se a msg começar com :info
Commands.register(":info", true, function(){
	//Executa commando 
})
//Funciona como o ativador Usuáiro fala
Commands.register("info", false, function(){
	//Executa commando 
})
```
## Objeto: Faker
O objeto **Faker** é responsável por criar furnis "falsos" que só são visualizáveis 

### Funções e métodos:
```javascript
//Cria e retorna um FakeItem
Faker.createFakeItem(spriteId, x,y,z,r)
//Remove um FakeItem especifico
Faker.removeFakeFloorItem(fakeItem)
//Remove todos os FakeItems do quarto
Faker.removeAllFloorItems()
//Retorna Array com todos os FakeItems
Faker.getLoadedFurnis()
```
## Persistência
Toda a informação persistida tem que ser uma string e a key não pode conter mais do que 100 caracteres
### Global
As keys são compartilhadas por todos os quartos do usuário
```javascript
GlobalStorage.set(key,value)
GlobalStorage.get(key)
GlobalStorage.delete(key)
```

### Quarto 
As keys só são visíveis dentro do quarto
```javascript
RoomStorage.set(key,value)
RoomStorage.get(key)
RoomStorage.delete(key)
```
