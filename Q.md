

```scala211
def produceArrayN(n: Int) = {
    def fifthyFifthyRat = if(scala.util.Random.nextBoolean) 1 else 0
    Array.fill(n)(fifthyFifthyRat)
}

implicit class Entangle(b: Array[Int]) {

  def entangleWith(a: Array[Int]): Array[Int] = {
    var bbb = b.toList
    def pull(value: Int) = {
      val i = Option(bbb.indexWhere(_ == value)).filter(_ != -1).getOrElse(0)
      val res = bbb(i)
      bbb = bbb.patch(i, Nil, 1) //remove i-th element from bbb
      res
    }
    val newb = (a zip b).map{
        case (aa, bb) => 
        if (aa == bb && aa == 1) pull(0) else pull(bb)
    }
    newb
  }

}


def analyze(n: Int) = {
 
    def mean(aa: Array[Int]) = aa.count(_ == 1).toDouble / aa.size
    def means(aa: Array[(Int, Int)]) = mean(aa.map(_._1)) -> mean(aa.map(_._2))

    def score(aa: Array[(Int, Int)]) = aa.count{case (a,b) => a * b == 0}.toDouble / aa.size
    
    case class Report(entangledE: (Double, Double), originalE: (Double, Double), entangledScore: Double, originalScore: Double)    
    
    val left = produceArrayN(n)
    val right = produceArrayN(n)
  
    val entRight = right entangleWith left
    val entangled = left zip entRight
    val original = left zip right
    
    Report(means(entangled), means(original), score(entangled), score(original))
}
```




    defined [32mfunction[39m [36mproduceArrayN[39m
    defined [32mclass[39m [36mEntangle[39m
    defined [32mfunction[39m [36manalyze[39m




```scala211
import $ivy.`org.plotly-scala::plotly-jupyter-scala:0.3.0`

import plotly._
import plotly.element._
import plotly.layout._
import plotly.JupyterScala._

plotly.JupyterScala.init()

val epochs = 128
val reports = (1 to epochs).toList.map(_ => analyze(9036))

val step = 0.01

val toPlot = Map(
    "entangledLeft" -> reports.map(_.entangledE._1),
    "originalLeft" -> reports.map(_.originalE._1),
    "entangledRight" -> reports.map(_.entangledE._2),
    "originalRight" -> reports.map(_.originalE._2)
)

val plot = toPlot map { 
    case (name, ent) =>
        val x = (0.1 to 1.0 by step).toList
        val y = x.map{xx =>
          ent.count(m => m > xx && m < xx + step)   
        }
        Scatter(x, y, name = name)  
}

plot.toList.plot()

val plot2 = List(
    Scatter(1 to epochs, reports.map(_.entangledScore), name = "entangled"),
    Scatter(1 to epochs, reports.map(_.originalScore), name = "original")
)
    
plot2.plot()

val entangledScore = reports.map(_.entangledScore).sum / epochs
val originalScore = reports.map(_.originalScore).sum / epochs

println("Average score:" + entangledScore + " compared to original " + originalScore)
```



      <script type="text/javascript">
        require.config({
  paths: {
    d3: 'https://cdnjs.cloudflare.com/ajax/libs/d3/3.5.17/d3.min',
    plotly: 'https://cdn.plot.ly/plotly-1.12.0.min'
  },

  shim: {
    plotly: {
      deps: ['d3', 'jquery'],
      exports: 'plotly'
    }
  }
});
        

        require(['plotly'], function(Plotly) {
          window.Plotly = Plotly;
        });
      </script>
    



<div class="chart" id="plot-381049847"></div>





<div class="chart" id="plot-1730584006"></div>




    Average score:0.8329935466467464 compared to original 0.7494769187140329





    [32mimport [39m[36m$ivy.$                                             
    
    [39m
    [32mimport [39m[36mplotly._
    [39m
    [32mimport [39m[36mplotly.element._
    [39m
    [32mimport [39m[36mplotly.layout._
    [39m
    [32mimport [39m[36mplotly.JupyterScala._
    
    [39m
    [36mepochs[39m: [32mInt[39m = [32m128[39m
    [36mreports[39m: [32mList[39m[[32mReport[39m forSome { type [32mReport[39m <: [32mAnyRef[39m with [32mProduct[39m with [32mSerializable[39m{val entangledE: (Double, Double);val originalE: (Double, Double);val entangledScore: Double;val originalScore: Double;def copy(entangledE: (Double, Double),originalE: (Double, Double),entangledScore: Double,originalScore: Double): Report;def copy$default$1: (Double, Double);def copy$default$2: (Double, Double);def copy$default$3: Double;def copy$default$4: Double} }] = [33mList[39m(
      Report((0.5025453740593183,0.4953519256308101),(0.5025453740593183,0.4953519256308101),0.8362107127047366,0.7504426737494466),
      Report((0.49922532093846833,0.4950199203187251),(0.49922532093846833,0.4950199203187251),0.8385347498893315,0.7537627268702966),
      Report((0.49369189907038513,0.5073041168658698),(0.49369189907038513,0.5073041168658698),0.835436033643205,0.7491146525011066),
      Report((0.4960159362549801,0.5044267374944665),(0.4960159362549801,0.5044267374944665),0.8352146967684816,0.7547587428065515),
      Report((0.50066401062417,0.5022133687472333),(0.50066401062417,0.5022133687472333),0.834108012394865,0.7487826471890217),
      Report((0.49424524125719344,0.49778663125276673),(0.49424524125719344,0.497786[33m...[39m
    [36mstep[39m: [32mDouble[39m = [32m0.01[39m
    [36mtoPlot[39m: [32mMap[39m[[32mString[39m, [32mList[39m[[32mDouble[39m]] = [33mMap[39m(
      [32m"entangledLeft"[39m -> [33mList[39m(
        [32m0.5025453740593183[39m,
        [32m0.49922532093846833[39m,
        [32m0.49369189907038513[39m,
        [32m0.4960159362549801[39m,
        [32m0.50066401062417[39m,
        [32m0.49424524125719344[39m,
        [32m0.50066401062417[39m,
        [32m0.5064187693669765[39m,
        [32m0.4995573262505533[39m,
        [32m0.5034307215582116[39m,
    [33m...[39m
    [36mplot[39m: [32mcollection[39m.[32mimmutable[39m.[32mIterable[39m[[32mScatter[39m] = [33mList[39m(
      [33mScatter[39m(
        [33mSome[39m(
          [33mDoubles[39m(
            [33mList[39m(
              [32m0.1[39m,
              [32m0.11[39m,
              [32m0.12[39m,
              [32m0.13[39m,
              [32m0.14[39m,
              [32m0.15000000000000002[39m,
              [32m0.16000000000000003[39m,
    [33m...[39m
    [36mres13_11[39m: [32mString[39m = [32m"plot-381049847"[39m
    [36mplot2[39m: [32mList[39m[[32mScatter[39m] = [33mList[39m(
      [33mScatter[39m(
        [33mSome[39m(
          [33mDoubles[39m(
            [33mVector[39m(
              [32m1.0[39m,
              [32m2.0[39m,
              [32m3.0[39m,
              [32m4.0[39m,
              [32m5.0[39m,
              [32m6.0[39m,
              [32m7.0[39m,
    [33m...[39m
    [36mres13_13[39m: [32mString[39m = [32m"plot-1730584006"[39m
    [36mentangledScore[39m: [32mDouble[39m = [32m0.8329935466467464[39m
    [36moriginalScore[39m: [32mDouble[39m = [32m0.7494769187140329[39m




```scala211

```
