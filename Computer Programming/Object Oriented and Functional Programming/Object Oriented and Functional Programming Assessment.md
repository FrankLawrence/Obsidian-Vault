Your final grade is calculated as follows:

```scala
def courseGrade(warmUpGrade : Double, // 1  
               snakeGrade : Double, // 2.3  
               tetrisGrade : Double, // 3.2  
               replGrade   : Double, // 4.2  
               bonusGrade : Option[Double],  
               examGrade : Double): Double = {  
    val prelimAssignmentGrade =  
        warmUpGrade / 10.0 * 1.0 +  
        snakeGrade  / 10.0 * 2.25 +  
        tetrisGrade / 10.0 * 2.25 +  
        replGrade   / 10.0 * 3.5 +  
        bonusGrade.getOrElse(0.0) / 10.0 * 2.0  
    val prelimGrade = (prelimAssignmentGrade * 0.6 + examGrade * 0.4)  
    val passAssignments =  
	    warmUpGrade >= 5.5 &&  
        snakeGrade >= 5.5 &&  
        tetrisGrade >= 5.5 &&  
        replGrade >= 5.5  
    val passExam = examGrade >= 5.5  
    val mayPass = passAssignments && passExam  
    if(mayPass) prelimGrade  
    else Math.min(5.4,prelimGrade)  
}
```

There is no resit opportunity for the assignments aside from the asynchronous submission system.

The grades for the exam and your VUNet grade will be posted as grades for canvas assignments. Note that canvas also displays some percentage, which may seem to indicate your final grade but does not.