# Javascript Snippets

## fabric.js: transform local to absolute coordinates of related object

### Requirement

* require matrix library [Sylvester](http://sylvester.jcoglan.com/)
* require fabric object property originX and originY equal to 'center'

### Code

```javascript
getAbsolutePosition=function (relatedObj, lpoint) {
  var alp = $V([lpoint.x, lpoint.y, 1.0]);        
  var co = relatedObj;
  var mt,st,rt, comb,fcomb=Matrix.I(3);
  while (co !== undefined) {
    mt = $M([
      [1, 0, co.left],
      [0, 1, co.top],
      [0, 0, 1]
    ]);
    st = $M([
      [co.scaleX, 0,0],
      [0, co.scaleY, 0],
      [0, 0, 1]
    ]);
    rt = Matrix.RotationZ(co.angle / 180.0 * Math.PI);
    comb = mt.x(rt.x(st));
    fcomb = comb.x(fcomb);
    co = co.group;
  }
  result = fcomb.x(alp);
  return result;
}
```

## fabric.js: transform absolute to local coordinates of related object

### Requirement

* matrix library [Sylvester](http://sylvester.jcoglan.com/)
* fabric object property originX and originY equal to 'center'

### Code

```javascript
getLocalPosition= function (relatedObj, apoint) {
  var vlp = $V([apoint.x, apoint.y, 1.0]);
  var co = relatedObj;
  var mt, st,rt, comb,fcomb=Matrix.I(3);
  while (co !== undefined) {
    mt = $M([
      [1, 0, -co.left],
      [0, 1, -co.top],
      [0, 0, 1]
    ]);
    st = $M([
      [1.0/co.scaleX, 0,0],
      [0, 1.0/co.scaleY, 0],
      [0, 0, 1]
    ]);
    rt = Matrix.RotationZ(-co.angle / 180.0 * Math.PI);
    comb = st.x(rt.x(mt));
    fcomb = fcomb.x(comb);
    co = co.group;
  }
  result = fcomb.x(vlp);
  return result;
}
```