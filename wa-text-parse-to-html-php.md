
```
<?php
class WaParse 
{
    public function parse($string){
        return $this->replace_to_html_format($string);
    }

    protected function replace_to_html_format($string)
    {
        $string = str_replace("\\n","<br/>",$string);
        for($i = 0; $i<strlen($string); $i++){
            foreach($this->parseArry() as $key => $val){
                $from = $val['from'];
                $end = $val['end'];
    
                $to  = $val['to'];
                $do = $val['do'];
                
                $pattern = "/{$from}(.*?){$end}/s";
    
                if( preg_match($pattern, $string, $match) && $match ){
                    $string = str_replace( trim($match[0]), $this->replace_to(trim($match[0]),$from,$end,$to,$do) ,$string);
                }
            }
        }

        // $string = str_replace("¶","<br/>",$string);
        return $string;
    }

    protected function replace_to($string,$from,$end,$to,$do)
    {
        // change char to tag html
        $string = preg_replace("/(^{$from})/s",$to,$string);
        $string = preg_replace("/({$end}$)/s",$do,$string);

        return $string;
    }

    protected function parseArry()
    {
        return [
            [ 'from' => "\\*", 'end' => "\\*", 'to' => "<b>", 'do' => "</b>" ],
            [ 'from' => "_", 'end' => "_", 'to' => "<i>", 'do' => "</i>" ],
            [ 'from' => "~", 'end' => "~", 'to' => "<del>", 'do' => "</del>" ],
            [ 'from' => "```", 'end' => "```", 'to' => "<code>", 'do' => "</code>" ],
        ];
    }

}

$test = new WaParse;
$string = "
    hello \n
    
    *hello* \n
    *_hello_* \n
    *~hello~* \n

    ~hello~ \n
    ~*hello*~ \n
    ~_hello_~ \n

    _hello_ \n
    _*hello*_ \n
    _~hello~_ \n

    *~_hello_~* \n
    *_~hello~_* \n

    ~_*hello*_~ \n
    ~*_hello_*~ \n

    _~*hello*~_ \n
    _*~hello~*_ \n

";

echo ($test->parse($string));
$time_start = microtime(true); 

//sample script
for($i=0; $i<1000; $i++){
    //do anything
}
   
$time_end = microtime(true);

//dividing with 60 will give the execution time in minutes otherwise seconds
$execution_time = ($time_end - $time_start)/60;

//execution time of the script
echo '<b>Total Execution Time:</b> '.$execution_time.' Mins';

