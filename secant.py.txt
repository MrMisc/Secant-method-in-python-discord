
@client.command()
async def secant(ctx, arg1):
    F = arg1.strip()
    def f(x):
        y = ne.evaluate(F)
        return y
    await ctx.send("Between what value limits would you presume your solution lies?")
    limits = await client.wait_for('message', timeout=30.0, check = lambda message: message.author == ctx.author)
    limits_string = limits.content

    vals = list(map(float,limits_string.split()))
    x1 = (vals[0])
    x2 = (vals[1])

    await ctx.send("How many iterations do you wish to allow?")
    max_ite = await client.wait_for('message', timeout = 30.0, check = lambda message: message.author == ctx.author)
    nMax = int(max_ite.content)

    await ctx.send("Finally, what level of tolerance do you have? What value would you consider small enough to be assumed to be 0?")
    tolerance = await client.wait_for('message', timeout = 30.0, check = lambda message: message.author == ctx.author)

    tol = ne.evaluate(tolerance.content)
    err = 1.0
    iteration = 0

    x = []
    x.append(x1)
    x.append(x2)
    while (err > tol) and iteration < nMax:
        xk = x1
        xk1 = x2
        iteration = iteration + 1
        err = xk1
        xk1 = xk - f(xk)*(xk-xk1)/(f(xk)-f(xk1))
        err = abs(err - xk1)
        x1 = x2
        x2 = xk1
        x.append(xk1)

    if iteration == nMax:
        await ctx.send('Maximum number of iterations reached. Change nMax within script to continue.')
        return

    await ctx.send(f'```Secant method reproduced a value of {xk}```')

