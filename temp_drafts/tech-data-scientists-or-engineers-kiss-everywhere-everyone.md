# Data scientists or engineers, KISS everywhere, everyone

Data scientists rush to make a quick model prototype for a client. Then, the bomb gets thrown over to engineers who put it to production. Somewhere during production, new bombs get thrown over, the cycle continues, and we all live happily ever after (not). This is probably a very common scenario, where the friction point are the bombs, consisting of prototype code that have the potential to give engineers headaches or nightmares.

The first thing I was taught when I moved into machine learning/software engineering was "Keep it Simple, Stupid" (KISS). The main goal of KISS is simply to avoid unnecessary complexity in the code. The straightforward benefit to whomever handles the code is that it should be easy to follow and understand. The beauty, which is always overlooked and underappreciated, is that very often, simple code is optimized naturally to some extent, and has a less chance of introducing bugs.

We are always rushed to develop code quickly to meet deadlines, which often means sloppy/convoluted code, translating to technical debt. A debt is a debt, and if it isn't paid, the "interest" or penalty is huge, and can easily sink a team. Engineering is to some extent "problem solving by KISS-ing". But why should KISS only apply to "engineering" or "production"? If everyone takes a little more time and effort and does KISS everywhere, this really can go a long way, and alleviate these friction points or "bombs" being thrown back and forth within teams

Below is some fictional scenario showing some dumb code to illustrate how KISS makes things better.

## Fictional example
### Scenario
- Say we have some sales data of 20 million items, with "columns" `selling_prices`, `costs_per_unit`, `unit_sold_counts`, e.g.:
  ![image](https://user-images.githubusercontent.com/44337585/156944778-27903a1a-ad90-4f65-b912-073e11fe3b8b.png)
- There might be better ways instead of a list to store/process data at this size, but for argument's sake, let's stick with 3 separate lists, so we get something like this:
  - ```
    selling_prices = [142, 52, 94, 72, 47, 86, ...]
    costs_per_unit = [21, 14, 26, 18, 27, 38, ...]
    units_sold_counts = [9090, 70, 300, 930, 204, 650, ...]
    ```
- We want to compute the `total_profit`, i.e. summing up the (`selling_price` - `cost_per_unit`) x `units` for each fruit

### Exploration
- For demonstration purposes, let's do something exaggeratedly dumb (i.e. rush and sloppy code):
  - Create an empty dataframe 
  - Insert the columns of information one by one
  - Compute the `total_profit` with `df.apply` + a `lambda` function and summing the results
  - Job done! (really?)
![image](https://user-images.githubusercontent.com/44337585/156944962-2f99bda0-e4af-4fd8-b825-1e38a8da8853.png)
  
### KISS
1. Simplify the code first by:
   - Creating the dataframe in one go
   - Using [broadcasting operations](https://numpy.org/doc/stable/user/basics.broadcasting.html) to compute the required output
   - Assert the output is consistent to the original `total_profit`

    ![image](https://user-images.githubusercontent.com/44337585/156945132-045d526d-eda5-4286-8f1f-4e7cd8b3a41a.png)

2. Think...why do we even need pandas? We can just compute the output directly, and is the code easier to understand?

    ![image](https://user-images.githubusercontent.com/44337585/156945203-b93aeb5b-ae81-41a4-80ac-349b11a5614c.png)

3. At this kind of scale, maybe using numpy could speed things up, but still keep the code simple, like this:
    ![image](https://user-images.githubusercontent.com/44337585/156945296-2edc0448-c151-48f2-817e-72dac28d1577.png)


### Timeit!
- Now let's time how long each method takes to run with IPython/jupyter notebook's [%timeit](https://ipython.readthedocs.io/en/stable/interactive/magics.html#magic-timeit) magic command
- Results (in seconds):
  - (Exaggeratedly dumb) original method: 280
  - Method with pandas (improved): 17.7
  - Method without pandas: 4.48
  - Method with numpy: 3.82
![image](https://user-images.githubusercontent.com/44337585/156946987-96652d64-9581-416a-ae4c-a99c7f480cc1.png)

  
# Thoughts

The results are pretty clear. The last two methods are simpler and faster. Had we put the original code into production, we would then have to refactor it later when we hit scaling problems. This is just a small change where you strip out pandas, simply because you do not need to create the whole dataframe (which is a big overhead, even if refactored!) before computing the results. These are tiny details, but every little details somewhat counts.

I am not here to criticise pandas, as it has its place for data exploration! What I wanted to say is that KISS should really be applied AT ALL TIMES, BY EVERYONE. By stripping out anything you don't really need:
- The entire developer's experience will be naturally improved 
- Teams will have less technical debt accumulating over time
- Code will most likely run better
- There's a reduced chance of introducing bugs!
- Simple = less dumb (most of the time)

"That is all!" (quote from a team member whenever he feels like he's said something sensible)

Note: the original working notebook can be found [here](https://github.com/chilledgeek/blog_dump/blob/main/fictional_scenarios/fictional_sales.ipynb)
