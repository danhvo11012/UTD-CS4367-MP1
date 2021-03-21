# UTD-CS4367-MP1
Repos for CS4367

## Part 1:
Method __doAnalysis__ has been implemented as in __DominatorFinder.java__. The method did the job decently and precisely.


## Part 2:
###### Comparing Different Call Graph Construction ALgorithms
We're provided TestSootCallGraph.java and Example.java. The comparison is as follows:

The attempt to execute TestSootCallGraph.java without enabling CHA and PTA resulted in the __Total Edges = 12__ and took __2 seconds__.
When CHA mode was on (enableCHACallGraph()), the attempt resulted in __Total Edges = 12__ and also took __2 seconds__.
When PTA mode was on (enableSparkCallGraph()), the attemp resulted in __Total Edges = 7__ and took __3 seconds__.

###### Conclusion
For this particular case of Example.java, PTA performed is more precise (7 edges versus 12 edges), yet less efficient (2 seconds versus 3 seconds), in comparison with CHA

## Part 3:
###### Tracing Heap Accesses
TestSootLogging Heap.java was improvised so that internalTransform can produce desired output. The code is as follows:
```
	int isStatic = (stmt.getFieldRef().getClass().getName().equals("soot.jimple.StaticFieldRef"))? 1 : 0;
	String subClassName = stmt.getDefBoxes().get(0).getValue().getClass().getName();
	int isWrite = (subClassName.equals("soot.jimple.StaticFieldRef") || subClassName.equals("soot.jimple.internal.JInstanceFieldRef")) ? 1 : 0;
	String obj = "Thread[" + Thread.currentThread().getName()+","+Thread.currentThread().getPriority()+","+Thread.currentThread().getThreadGroup().getParent().getName()+"]";
		    		
	LinkedList<Value> args = new LinkedList<>();
	args.add(StringConstant.v(obj));
	args.add(StringConstant.v(stmt.getFieldRef().getField().toString()));
	args.add(IntConstant.v(isStatic));
	args.add(IntConstant.v(isWrite));
		    		
	InvokeExpr printExpr = Jimple.v().newStaticInvokeExpr(logFieldAccMethod, args);
	InvokeStmt invokeStmt = Jimple.v().newInvokeStmt(printExpr);
	b.getUnits().insertBefore(invokeStmt,stmt);```

