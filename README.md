# Ruler
刻度尺
## 纯vue3+swiper 写法，适合各个平台，包含H5  各种小程序，app 等
#  使用方法1：导入到uni_modules 下面后 直接调用方法
<cll-ruler
		  :defaultValue="68"
		  unit="kg"
		  color="#3bd7d0"
		  scaleColor="#cccccc"
		  @ruleChange="ruleChange"
		/>

##  使用方法2 局部引入  import cllRuler from "@/compents/cll-ruler/cll-ruler" (具体路径根据项目来)

 <cll-ruler
		  :defaultValue="68"
		  unit="kg"
		  color="#3bd7d0"
		  scaleColor="#cccccc"
		  @ruleChange="ruleChange"
		/>
