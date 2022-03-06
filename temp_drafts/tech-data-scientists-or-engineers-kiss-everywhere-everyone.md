# Data scientists or engineers, KISS everywhere, everyone

Data scientists rush to make a quick model prototype for a client. Then, the bomb gets thrown over to engineers who put it to production. Somewhere during production, new bombs get thrown over, the cycle continues, and we all live happily ever after (not). This is probably a very common scenario, where the friction point are the bombs, consisting of prototype code that have the potential to give engineers headaches or nightmares.

The first thing I was taught when I moved into machine learning/software engineering was "Keep it Simple, Stupid" (KISS). The main goal of KISS is simply to avoid unnecessary complexity in the code. The straightforward benefit to whomever handles the code is that it should be easy to follow and understand. The beauty, which is always overlooked and underappreciated, is that very often, simple code is optimized naturally to some extent, and has a less chance of introducing bugs.

We are always rushed to develop code quickly to meet deadlines, which often means sloppy/convoluted code, translating to technical debt. A debt is a debt, and if it isn't paid, the "interest" or penalty is huge, and can easily sink a team. Engineering is to some extent "problem solving by KISS-ing". But why should KISS only apply to "engineering" or "production"? If everyone takes a little more time and effort and does KISS everywhere, this really can go a long way, and alleviate these friction points or "bombs" being thrown back and forth within teams

Below is some fictional scenario showing some dumb code to illustrate how KISS makes things better.

## Fictional example
### Scenario
- Say we have some sales data from selling exotic fruits, with "columns" `fruit_name`, `selling_prices`, `costs_per_unit`, `unit_sold_counts`
- We want to compute a list of `profit_per_fruit`, i.e. (`selling_price` - `cost_per_unit`) x `units` for each fruit

### Exploration
- For demonstration purposes, let's do something exaggeratedly dumb and create an empty dataframe and one by one insert the columns of information (i.e. rush and sloppy code)
- Compute the `profit_per_fruit` with `df.apply` + a `lambda` function
- Job done! (really?)
![image](https://user-images.githubusercontent.com/44337585/156896469-cd9a7f15-153a-455c-82c3-dc3b081a34e0.png)

### KISS
1. Simplify the code first by:
   - Creating the dataframe in one go
   - Using [broadcasting operations](https://numpy.org/doc/stable/user/basics.broadcasting.html) to compute the required output
  
    ![image](https://user-images.githubusercontent.com/44337585/156896740-d7d717f8-bf52-485a-a4d4-bfffa4c7a33d.png)
2. Think...why do we even need pandas? We can just compute the output directly, and is the code easier to understand?
![image](https://user-images.githubusercontent.com/44337585/156896762-e7ff61df-eb7a-4f79-979f-5f7eb4281733.png)

### Time it
- Now we have 3 methods that do the same thing, let's time it with IPython/jupyter notebook's [%timeit](https://ipython.readthedocs.io/en/stable/interactive/magics.html#magic-timeit) magic command
- Results (in microseconds):
  - (Exaggeratedly dumb) original method: 1780
  - Slightly optimised method with dataframes: 503
  - Method without pandas: 1
  
  ![image](https://user-images.githubusercontent.com/44337585/156896836-8e39de67-a11a-4500-a277-b4cb837c80b1.png)

# Thoughts
![dumb_sketch](https://user-images.githubusercontent.com/44337585/156897933-f2744943-17ca-4ada-b838-2abbd527442f.svg)

Apologies to readers who think that this is obvious, but the results are pretty clear. The simple code is simpler and faster. Although the units are in microseconds, the 500 times improvement in performance is significant when we talk about scaling up these calculations, so why not? Had we put the original code into production, we would then have to refactor it later when we hit scaling problems. Yes, this is just a TINY technical debt. But you know what? Every little counts.

I am not here to criticise pandas, as it has its place for data exploration! What I wanted to say is that KISS should really be applied AT ALL TIMES, BY EVERYONE. By stripping out anything you don't really need:
- The entire developer's experience will be naturally improved 
- Teams will have less technical debt accumulating over time
- Code will most likely run better
- There's a reduced chance of introducing bugs!

We all do dumb things from time to time (I'll be the first to admit to this). The most important thing is through KISS, learning and adapting, we can strip out the dumb things over time.

"That is all!" (quote from a team member whenever he feels like he's said something sensible)

