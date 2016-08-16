# Add monoticity as constraint to cost function

## tensorflow

    def monotonically_increasing(cost, var):
        for i in range(1, int(var.get_shape()[0])):
            cost += tf.cond(tf.less_equal(var[i-1], var[i]),
                            lambda: tf.constant(0, dtype=tf.float32),
                            lambda: tf.constant(10000, dtype=tf.float32))
    
        return cost
