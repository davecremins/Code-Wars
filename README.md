# Code-Wars
Solutions to some of the code-wars problems I've tackled

- [Sum Strings as Numbers](https://www.codewars.com/kata/5324945e2ece5e1f32000370)
```javascript
function sumStrings(a,b) {
  a = a.replace(/^0+/, '');
  b = b.replace(/^0+/, '');
  let smallest = a.length > b.length ? b.split('').reverse() : a.split('').reverse(),
      largest = b.length >= a.length ? b.split('').reverse() : a.split('').reverse(),
      lengthDiff = largest.length - smallest.length;
  
  while(lengthDiff > 0) {
    smallest.push('0');
    lengthDiff--;
  }
  
  let carry = 0, result = [], maxSize = largest.length-1;
  for(let i = 0; i <= maxSize; i++) {
    let sum = parseInt(largest[i], 10) + parseInt(smallest[i], 10) + carry, remainder = sum%10;
    carry = Math.floor(sum/10);
    result.push(remainder);
    if(i === maxSize && carry > 0) result.push(carry);
  }
  return result.reverse().join('');
}
```
- [Rot13](https://www.codewars.com/kata/530e15517bc88ac656000716)
```javascript
function rot13(message){
  let rotate = (x) => {
    const rotation = 13;
    let char = x.charCodeAt(0), rotated = char + rotation, diff = 0, lowBound = 64, highBound = 90;
    if((char >= 97) && (char <= 122)){
      lowBound = 96;
      highBound = 122;
    }
    
    if(rotated <= highBound) return rotated;
    
    char = lowBound;
    diff = rotated - highBound;
    return char + diff;
  };
  
  return message.split('').map((x) => { return /[a-zA-Z]/.test(x) ? String.fromCharCode(rotate(x)) : x; }).join('');
}
```
- [Breadcrumb Maker](https://www.codewars.com/kata/563fbac924106b8bf7000046)
```javascript
let condense = (text) => {
   if(text.length <= 30) return text;
   let blacklist = ["the","of","in","from","by","with","and", "or", "for", "to", "at", "a"];
   return text.split('-').map((x) => { return !blacklist.includes(x) ? x[0] : ''; }).join('');
};

let createAnchor = (href, text) => {
   return `<a href="/${href}">${text}</a>`;
};

let createSpan = (text) => {
   return `<span class="active">${text}</span>`;
};

let buildBreadCrumb = (index, end, text, previousText, separator) => {
   if(index === end) {
      let spanText = condense(text).replace(/-/g, ' ');
      spanText = (~spanText.indexOf('.')) ? spanText.substring(0, spanText.indexOf('.')) : spanText;
      return createSpan(spanText.toUpperCase());
   } 

   let href = (index !== 0) ? previousText + text + '/' : '';
   let anchorText = condense(text).replace(/-/g, ' ').toUpperCase();
   return createAnchor(href, anchorText) + separator;
};

function generateBC(url, separator) {
   url = url.replace(/https?:\/\//, '');
   const qMark = /^(.*?)(?=\?|$)/, hash = /^(.*?)(?=#|$)/;
   url = qMark.test(url) ? qMark.exec(url)[0] : url;
   url = hash.test(url) ? hash.exec(url)[0] : url;

   let urlParts = url.split('/');
   if(urlParts[urlParts.length-1].includes('index')) urlParts.splice(-1);
   if(urlParts[urlParts.length-1] === '') urlParts.splice(-1);
   urlParts[0] = 'home';

   let result = '', end = urlParts.length-1, previous = '';
   for(let i = 0; i < urlParts.length; i++) {
      if(i > 1) previous += urlParts[i-1] + '/';
      result += buildBreadCrumb(i, end, urlParts[i], previous, separator);
   }
   return result;
}
```
- [Unary Function Chainer](https://www.codewars.com/kata/54ca3e777120b56cb6000710)
```javascript
function chained(functions) {
  return function(x){
    return functions.reduce((x, f) => { return f(x); }, x);
  };
}
```
