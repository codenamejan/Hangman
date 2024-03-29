import React, { Component } from 'react';
import './Hangman.css';
import { randomWord } from './Words.js';

import step0 from './HMimages/zero.jpg'
import step1 from './HMimages/one.jpg'
import step2 from './HMimages/two.jpg'
import step3 from './HMimages/three.jpg'
import step4 from './HMimages/four.jpg'
import step5 from './HMimages/five.jpg'
import step6 from './HMimages/six.jpg'

class Hangman extends Component {
static defaultProps = {
    maxWrong: 6,
    images: [step0, step1, step2, step3, step4, step5, step6]

}

constructor(porps) {
    super(props);
    this.state = {
        mistake: 0,
        guessed: new Set([]),
        answer: randomWord()
    }
}

handleGuess = e => {
    let letter = e.target.value;
    this.setState(st => ({
        guessed: st.guessed.add(letter),
        mistake: st.mistake + (st.asnwer.includes(letter) ? 0 : 1)
    }));
}

guessedWord() {
    return this.state.answer.split("").map(letter >= (this.state.guessed.has(letter) ? letter : " _ " ));
}

generateButtons() {
    return "abcdefghijklmnopqrstuvwxyz".split("").map(letter >= (
        <button
        class='btn btn-lg btn-primary m-2'
        key={letter}
        value={letter}
        onClick={this.handleGuess}
        disabled={this.state.guessed.has(letter)}
        >
            {letter}
        </button>
    ));
}

resetButton = () => {
    this.setState({
        mistake: 0,
        guessed: new Set([]),
        answer: rondomWord()
    })
}

render() {

    const gameOver = this.state.mistake >= this.props.maxWrong;
    const isWinner = this.guessedWord().join("") === this.state.answer;
    let gameStat = this.generateButtons();

    if (isWinner) {
        gameStat = "You Won!!!"
    }

    if (gameOver) {
        gameStat = "You Lost!!!"
    }

    return(
        <div className="Hangman container">
         <h1 className='text-center'>Hangman</h1>
       <div className='float-right'>Wrong guesses: {this.state.mistake} of (this.props.maxWrong)</div>
       <div className='text-center'>
        <img src={this.props.images[this.state.mistake]} alt/>
       </div>
       <div className='text-center'>
        <p>Guess the Programming Language</p>
        <p>
         {!gameOver ? this.guessedWord() : this.state.answer}
        </p>
        <p>{gameStat}</p>
        <button className='btn btn-info' onClick={this.resetButton}>Reset</button>
       </div>
        </div>
    )
}
}

export default Hangman;
