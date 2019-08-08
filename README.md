# pytorch-hacks

PyTorch Hackathon Repo

# API Demo

~~~python
import learn2learn as l2l

task_generator = l2l.data.TaskGenerator(MNIST, ways=3)
model = Net()
maml = l2l.MAML(model, lr=1e-3)
opt = optim.Adam(maml.parameters(), lr=4e-3)

for iteration in range(num_iterations):
    learner = maml.new()  # Creates a clone of model
    task = task_generator.sample(shots=1)

    # Fast adapt
    for step in range(adaptation_steps):
        error = sum([loss(learner(X), y) for X, y in task])
        error /= len(task)
        learner.adapt(error)

    # Compute validation loss
    valid_task = task_generator.sample(shots=1, labels=task.labels)
    valid_error = sum([loss(learner(X), y) for X, y in valid_task])
    valid_error /= len(valid_task)

    # Take the meta-learning step
    opt.zero_grad()
    adapt_error.backward()
    opt.step()
~~~

# Acknowledgements

1. The RL environments are copied from: https://github.com/tristandeleu/pytorch-maml-rl
