#Distributed-Memory Programming with MPI

##Introduction to MPI

**Distributed Memory System**: a collection of core-memory pairs connected by a network

**Process**: a program running on a core-memory pair. One process calls a *send* fucntion and the others call *receive* function.

**MPI (Memory-Passing Interface)**

## Compilation and Execution

    mpicc -g -Wall -o mpi_hello mpi_hello.c
    mpiexec -n <num of processes> ./mpi_hello

##MPI\_Init and MPI\_Finalize##

    int MPI_Init(int* argc_p, char*** argv_p);

`argc_p` and `argv_p` are pointers to the arguments of `main`

    int MPI_Finalize(void);

##MPI\_Comm\_size and MPI\_Comm\_rank##

In MPI a **communicator** is a collection of processes that can send messages to each other.

    int MPI_Comm_size(
        MPI_Comm comm /*in*/,
        int* comm_sz_p /*out*/
    );

`MPI_Comm_size` get the number of processes in this communicator

    int MPI_Comm_rank(
        MPI_Comm comm /*in*/,
        int* my_rank_p /*out*/
    );

get the *rank* of the calling process

##SPMD programs

Most MPI programs are written in this way  
> a single program is written so that different processes carry out different actions 

having processes branch on the basis of their process rank

**single program, multiple data (SPMD)**

##MPI\_Send and MPI\_Recv

    int MPI_Send(
        void* msg_buf_p     /*pointer to the contents of the message*/,
        int msg_size        /*number of objects in the message*/,
        MPI_Datatype msg_type /*predefined MPI Datatypes*/,
        int dest            /*rank of the destination process*/,
        int tag             /*tag of the message, nonnegative int to determine the order of messages*/,
        MPI_Comm communicator /*communicator*/
    );

    int MPI_Recv(
        void* msg_buf_p     /*pointer to the receiving memory block*/,
        int buf_size        /*number of objects in the messages*/,
        MPI_Datatype msg_type /*MPI Datatypes*/,
        int source,
        int tag,
        MPI_Comm communicator,
        MPI_Status status_p /*out*/
    );

##Message matching 

    MPI_Send(send_buf_p, send_buf_sz, send_type, dest, send_tag, send_comm); //process q
    MPI_Recv(recv_buf_p, recv_buf_sz, recv_type, src, recv_tag, recv_comm, &status); //process r

The message sent from `q` is successfully received by `r` if

* `recv_comm` = `send_comm`
* `recv_tag` = `send_tag`
* `dest = r`
* `src = q`
* `recv_type = send_type` and `recv_buf_sz >= send_buf_sz`

Since the order of source of coming messages is indeterminate, MPI provides a special constant `MPI_ANY_SOURCE` that can be passed to `MPI_Recv`

Since the order of tag of coming messages is indeterminate, MPI provides a special constant `MPI_ANY_TAG` that can be passed to `MPI_Recv`

1. Only a receiver can use a wildcard argument. Senders must specify a process rank and a nonnegative tag. Thus MPI uses a *push* communication mechanism rather than a "pull" mechanism.
2. Both the senders and receivers must always specify communicators.

##The status_p argument: src, tag, number
The last argument to `MPI_Recv` has type `MPI_Status*`. The `MPI_status` is a struct with at least three members `MPI_SOURCE`, `MPI_TAG`, `MPI_ERROR`  
We can determine the sender and tag by examining the two members
 
    status.MPI_SOURCE
    status.MPI_TAG

The number of objects can be retrieved by `MPI_Get_count`

    int MPI_Get_count(
        MPI_Status* status_p /*in*/,
        MPI_Datatype type   /*in*/,
        int* count_p        /*out*/
    );

##MPI_Reduce
In MPI parlance, communication functions that involve all the processes in a communicator are called **collective communications**.   
To distinguish between collective communications and functions such as `MPI_Send` and `MPI_Recv`, `MPI_Send` and `MPI_Recv` are often called **point-to-point** communications.

    int MPI_Reduce(
        void* input_data_p,
        void* output_data_p,
        int count,
        MPI_Datatype datatype,
        MPI_Op operator  /*predefined reduction operations*/,
        int dest_process,
        MPI_Comm comm
    )

    MPI_Reduce(&local_int, &total_int, 1, MPI_DOUBLE, MPI_SUM, 0, MPI_COMM_WORLD);
