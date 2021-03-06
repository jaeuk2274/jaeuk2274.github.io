---
title: "[Angular]ng-grid 조회 타입별 칼럼 재빌드" 
date: 2019-12-12
categories:
- Front-end
tags:
- Angular 
- ng-grid
- colum
comments : true
---


## ng-grid 조회 타입별 칼럼 재빌드            

칼럼 몇개 반영할때마다             
1. 칼럼명으로 순서 인덱스  
2. if    
3. else    
총 3군데 다 반영해주어야 하는게 맘에 들지 않아서 개선해보았다.           


### 기존
```javascript
$scope.buildColumns = function(){
  var colMatchMesId = $scope.gridOptions.columnDefs.map(function (e) { return e.field; }).indexOf('matchMesId');
  var colPrice = $scope.gridOptions.columnDefs.map(function (e) { return e.field; }).indexOf('price');
  var colDeleteFlag = $scope.gridOptions.columnDefs.map(function (e) { return e.field; }).indexOf('deleteFlag');
  var colRemarks = $scope.gridOptions.columnDefs.map(function (e) { return e.field; }).indexOf('remarks');
  var colTestClass = $scope.gridOptions.columnDefs.map(function (e) { return e.field; }).indexOf('testClass');

  if($scope.searchMap.type == "unMatch"){
    $scope.gridOptions.columnDefs[colPrice].visible = false;
    $scope.gridOptions.columnDefs[colDeleteFlag].visible = false;   
    $scope.gridOptions.columnDefs[colRemarks].visible = false;  
    $scope.gridOptions.columnDefs[colMatchMesId].visible = false;  
    $scope.gridOptions.columnDefs[colTestClass].visible = false;  
  }else if($scope.searchMap.type == "data"){
    $scope.gridOptions.columnDefs[colPrice].visible = true;
    $scope.gridOptions.columnDefs[colDeleteFlag].visible = true; 
    $scope.gridOptions.columnDefs[colRemarks].visible = true;  
    $scope.gridOptions.columnDefs[colMatchMesId].visible = true;  
    $scope.gridOptions.columnDefs[colTestClass].visible = true;  
  } 

  $scope.gridOptions.ngGrid.buildColumns(); 
};
```        

### 개선
```javascript
$scope.buildColumns = function(){
  var colArr = ['matchMesId', 'price', 'deleteFlag', 'serviceCd', 'serviceNm', 'startDate', 'endDate', 'remarks', 'testClass']; // 데이터관리
  var colArr2 = ['pruefdatuv', 'insptDt', 'cnt']; // 미매칭
  var colIdxArr = [];
  var colIdxArr2 = [];

  colArr.forEach(function(item, index, array) {
    colIdxArr.push($scope.gridOptions.columnDefs.map(function (e) { return e.field; }).indexOf(item))
  });
  colArr2.forEach(function(item, index, array) {
    colIdxArr2.push($scope.gridOptions.columnDefs.map(function (e) { return e.field; }).indexOf(item))
  });

  colIdxArr.forEach(function(item, index, array) {
    $scope.gridOptions.columnDefs[item].visible = ($scope.searchMap.type == "data");
  });
  colIdxArr2.forEach(function(item, index, array) {
    $scope.gridOptions.columnDefs[item].visible = ($scope.searchMap.type == "unMatch");
  });

  $scope.gridOptions.ngGrid.buildColumns(); 
};
```

이제 위의 배열에만 칼럼명을 넣고 빼고 변경하면 된다.       

코드를 보면 알겠지만,       
1. 칼럼명 배열을 선언 후     

2. 해당 칼럼이 몇 번째에 존재하는지 찾고 칼럼idx 배열로 만든다. (칼럼이 몇 번째에 존재하는지 알아야 visible 설정 가능)   

3. 그리고 칼럼idx 배열만큼 돌리면서, 각 조회 타입에 따라 visible true / false 처리를 해 준다.    
(보였던 것은 타입 바꾸면 안보이게, 다시 바꾸면 보이게 처리. = ($scope.searchMap.type == "data") 내가 선택하는 조회 타입마다 바뀜).   

4. buildColumns으로 칼럼을 재빌드한다.      




