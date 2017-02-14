# RandomRedPackets
随机红包

```
<?php

/**
* 红包随机生成类
*/
class RandomRedPackets
{
    public $min = 6;
    public $max = 12;

    /**
     * 获取红包
     * @return [type] [description]
     */
    public function getRedPackets($total, $number)
    {
        // 判断红包是否能发完
        if( $number*$this->max < $total ){
            return false;
        }
        $baseArr = $this->generateBaseArr($this->min, $number);
        $randomArr = $this->generateBaseArr(0, $number);
        $remaining_total = $total-$this->min*$number;
        $this->generateRandomArr($randomArr, $remaining_total);
        foreach ($baseArr as $k => &$v) {
            $v += $randomArr[$k];
        }
        unset($v);
        shuffle($baseArr);
        return $baseArr;
    }

    /**
     * 根据金额及红包数量生成数组
     * @param  [type] $amount [description]
     * @param  [type] $number [description]
     * @return [type]         [description]
     */
    public function generateBaseArr($amount, $number)
    {
        $arr = [];
        for ($i=0; $i < $number; $i++) { 
            $arr[] = $amount;
        }
        return $arr;
    }

    /**
     * 获取红包随机数组
     * @param  [type] &$randomArr      [description]
     * @param  [type] $remaining_total [description]
     * @return [type]                  [description]
     */
    public function generateRandomArr(&$randomArr, $remaining_total)
    {
        $max = $this->max-$this->min;
        foreach ($randomArr as $k=>&$v) {
            $tmp = mt_rand(0, $max-$v);
            if($remaining_total-$tmp<=0){
                $v = $v+$remaining_total;
                return $randomArr;
            } else {
                $remaining_total -= $tmp;
            }
            $v = $v+$tmp;
        }
        unset($v);
        return $this->generateRandomArr($randomArr, $remaining_total);
    }

    /**
     * 设置红包最小金额
     * @param [type] $value [description]
     */
    public function setMin($value)
    {
        $this->min = $value;
    }

    /**
     * 设置红包最大金额
     * @param [type] $value [description]
     */
    public function setMax($value)
    {
        $this->max = $value;
    }
}

$obj = new RandomRedPackets;
$obj->setMin(0);
$result = $obj->getRedPackets(100, 10);
var_dump($result);
```

