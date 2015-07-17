**一列数，前两个都是1，从第三个开始，每个数都是前面两个数的和**
`方法一`

function getCurrentNum($num,$firstNum=1,$prevNum=0){

	while($num-- > 1){
		$tempNum = $firstNum;
		$firstNum = $firstNum + $prevNum;
		$prevNum = $tempNum;
		getCurrentNum($num,$firstNum,$prevNum);
	}
	return $firstNum;
}



`方法二`


function getCurrentNum($num){
    
	$startNum = 1; //第几个
	$firstNum = 1; 
	$prevNum = 0;
	while($startNum++ <= $num){
		getNum($firstNum,$prevNum);	
	}
	return $firstNum;
}

function getNum(&$firstNum,&$prevNum){

	$tempNum = $firstNum;
	$firstNum = $firstNum+$preNum;
	$preNum = $tempNum;
}


