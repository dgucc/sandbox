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

## Add Progress bar

@angular/material/progress-bar  
```
	ng add @angular/material

	> Indigo/Pink        [ Preview: https://material.angular.io?theme=indigo-pink ] 
	Deep Purple/Amber  [ Preview: https://material.angular.io?theme=deeppurple-amber ] 
	Pink/Blue Grey     [ Preview: https://material.angular.io?theme=pink-bluegrey ] 
	Purple/Green       [ Preview: https://material.angular.io?theme=purple-green ]

	? Set up global Angular Material typography styles? Yes
	? Set up browser animations for Angular Material? Yes
```
src/app/app.module.ts :  
```
	imports: [
		BrowserModule,
		AppRoutingModule,
		HttpClientModule,
		BrowserAnimationsModule,
		MatProgressBarModule
	],...
```
[TS] : define a flag "isLoading:boolean"  
	
	> src/app/sub-app1/sub-app1.component.ts  
	> src/app/sub-app2/sub-app2.component.ts  
	

[HTML] : use <map-progress-bar> linked to flag "isLoading" + *ngIf  

 > src/app/sub-app1/sub-app1.component.html  
 > src/app/sub-app2/sub-app2.component.html  
	
`	<mat-progress-bar mode="indeterminate" *ngIf="isLoading"></mat-progress-bar>`   


## d3 - angular

```
$ npm install d3 --save
$ npm install @types/d3 --save
app.component.ts : import * as d3 from 'd3'
```
