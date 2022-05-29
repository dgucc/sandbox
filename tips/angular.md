# Angular

## Installation steps

1. nodejs
```
$ curl -sL https://deb.nodesource.com/setup_14.x | sudo -E bash -`
$ sudo apt-get install -y nodejs
```

2. Update npm

`$ sudo npm install npm@latest -g`

3. Install the Angular CLI

`$ sudo npm install -g @angular/cli`

4. Create a workspace and initial Angular application
```
$ cd /home/user/workspace/angular
$ ng new sandbox
$ cd sandbox
$ ng generate component <component-name>
$ ng serve --open
```
## d3 - angular

```
$ npm install d3 --save
$ npm install @types/d3 --save
app.component.ts : import * as d3 from 'd3'
```