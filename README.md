import React, { Component } from 'react';  
class ApiDataFetcher extends Component { 
    state = { 
        data: [], 
        filteredData: [], 
        error: null, 
        searchQuery: '' 
    }; 
 
    componentDidMount() { 
        this.fetchData(); 
    } 
 
    fetchData = () => { 
        fetch('https://jsonplaceholder.typicode.com/posts')  
            .then((response) => response.json()) 
            .then((data) => { 
                this.setState({ data, filteredData: data }); 
            }) 
            .catch(() => { 
                this.setState({ error: 'Error fetching data' }); 
            }); 
    }; 
 
    componentDidUpdate(prevProps, prevState) { 
        if (prevState.searchQuery !== this.state.searchQuery) { 
            this.filterData(); 
        } 
    } 
 
    filterData = () => { 
        const { data, searchQuery } = this.state; 
        const filteredData = data.filter((item) => 
            item.title.includes(searchQuery) // Case-sensitive search 
        ); 
        this.setState({ filteredData }); 
    }; 
 
    handleSearchChange = (event) => { 
        this.setState({ searchQuery: event.target.value }); 
    }; 
 
    handleRefresh = () => { 
        this.fetchData(); 
    }; 
 
    render() { 
        const { filteredData, error, searchQuery } = this.state; 
 
        return ( 
            <div> 
                <h1>API Data Fetcher</h1> 
 
                {error && <p>{error}</p>} 
 
                <input 
                    type="text" 
                    placeholder="Search by title" 
                    value={searchQuery} 
                    onChange={this.handleSearchChange} 
                /> 
 
                <button onClick={this.handleRefresh}>Refresh Data</button> 
 
                {filteredData.length > 0 ? ( 
                    <table> 
                        <thead> 
                            <tr> 
                                <th>ID</th> 
                                <th>Title</th> 
                                <th>Body</th> 
                            </tr> 
                        </thead> 
                        <tbody> 
                            {filteredData.map((item) => ( 
                                <tr key={item.id}> 
                                    <td>{item.id}</td> 
                                    <td>{item.title}</td> 
                                    <td>{item.body}</td> 
                                </tr> 
                            ))} 
                        </tbody> 
                    </table> 
                ) : ( 
                     <p>No results found.</p> 
                )} 
            </div> 
        ); 
    } 
} 
 
export default ApiDataFetcher; 
