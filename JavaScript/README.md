# JavaScript记录
## 数组内的所有元素随机排列顺序
    if (!Array.prototype.shuffle) {
      Array.prototype.shuffle = function() {
        for(var j, x, i = this.length; i; j = parseInt(Math.random() * i), x = this[--i], this[i] = this[j], this[j] = x);
        return this;
      };
    }
    arr.shuffle();